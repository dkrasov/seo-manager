<template>
    <div>
        <div class="modal fade big-modal" :class="{show: showModal}" id="manager-modal" tabindex="-1" role="dialog">
            <div class="modal-dialog modal-dialog-centered modal-lg" role="document">
                <div class="modal-content">
                    <div class="modal-header">
                        <h5 class="modal-title">Keywords</h5>
                        <button type="button" @click="closeModal" class="close" data-dismiss="modal" aria-label="Close">
                            <span aria-hidden="true">&times;</span>
                        </button>
                    </div>
                    <div class="modal-body">
                        <form id="data-form">
                            <div class="form-row">
                                <div class="col-10 col-md-10 mb-10">
                                    <div class="form-group">
                                        <multi-select v-model="route.keywords" :options="route.keywords"
                                                      :taggable="true"
                                                      :close-on-select="false"
                                                      :show-labels="false"
                                                      placeholder="Keywords"
                                                      :multiple="true"
                                                      @tag="addKeyword">
                                        </multi-select>
                                    </div>
                                </div>
                                <div class="col-2 col-md-2 mb-2">
                                    <div class="form-group">
                                        <button type="button" @click="storeKeywords" :class="{'is-loading':saving}"
                                                class="btn btn-primary submit_button">Save Keywords
                                        </button>
                                    </div>
                                </div>
                            </div>
                        </form>
                    </div>
                </div>
            </div>
        </div>
        <div class="modal-backdrop fade" :class="{show: showModal}"></div>
    </div>
</template>

<script>
    import {EventBus} from "../../../../event_bus";

    export default {
        name: "KeywordsModal",
        props: {
            route: {
                type: Object
            },
            showModal: {
                type: Boolean
            },
            locale: {
                type: String
            }
        },
        data(){
            return{
                saving:false
            }
        },
        methods: {
            closeModal() {
                EventBus.$emit('close-modal', 'keywords');
            },
            addKeyword(keyword) {
                this.route.keywords.push(keyword);
            },
            storeKeywords() {
                this.saving = true;
                this.$http.post(API_URL + '/store-data?locale='+this.locale, {
                    id: this.route.id,
                    type: 'keywords',
                    keywords: this.route.keywords
                }).then(response => {
                    this.saving = false;
                    this.closeModal();
                })
            }
        }
    }
</script>

<style scoped>
    .modal-backdrop {
        z-index: -1;
        opacity: 0;
    }

    .modal-backdrop.show {
        z-index: 1040;
        opacity: .5;
    }

    .modal.show {
        opacity: 1 !important;
        display: block !important;
    }
</style>
