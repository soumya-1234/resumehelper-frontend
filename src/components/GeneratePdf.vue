<template>
    <div class="pdf-generator">
        <div class="container py-5">
            <!-- Header -->
            <div class="section-header text-center mb-5">
                <h1 class="display-5 mb-3">Generate Resume PDF</h1>
                <p class="lead text-muted">Create a professional PDF version of your resume</p>
            </div>

            <!-- PDF Controls -->
            <div class="controls-wrapper text-center mb-5">
                <button 
                    @click.prevent="generatePDF()" 
                    class="btn btn-primary btn-lg"
                    :disabled="isGenerating">
                    <i class="fas fa-file-pdf me-2"></i>
                    {{ isGenerating ? 'Generating PDF...' : 'Generate PDF' }}
                </button>
            </div>

            <!-- PDF Preview -->
            <div class="preview-wrapper">
                <div class="card border-0 shadow-sm">
                    <div class="card-header bg-primary text-white border-0 py-4">
                        <div class="d-flex align-items-center">
                            <div class="icon-circle me-3">
                                <i class="fas fa-file-alt fa-2x"></i>
                            </div>
                            <h2 class="h4 mb-0">Resume Preview</h2>
                        </div>
                    </div>

                    <div class="card-body p-4">
                        <vue-html2pdf
                            :show-layout="false"
                            :float-layout="true"
                            :enable-download="true"
                            :preview-modal="true"
                            :paginate-elements-by-height="1400"
                            filename="resume"
                            :pdf-quality="2"
                            :manual-pagination="false"
                            pdf-format="a4"
                            :pdf-margin="10"
                            pdf-orientation="portrait"
                            pdf-content-width="800px"
                            @progress="onProgress($event)"
                            @hasGenerated="onGenerated"
                            ref="html2Pdf"
                        >
                            <section slot="pdf-content">
                                <AllDetails/>
                            </section>
                        </vue-html2pdf>

                        <div class="preview-content">
                            <AllDetails/>
                        </div>
                    </div>
                </div>
            </div>
        </div>

        <!-- Progress Modal -->
        <div class="modal fade" :class="{ show: showProgress }" tabindex="-1" v-if="showProgress">
            <div class="modal-dialog modal-dialog-centered">
                <div class="modal-content">
                    <div class="modal-body text-center p-4">
                        <div class="progress-icon mb-3">
                            <i class="fas fa-spinner fa-spin fa-3x"></i>
                        </div>
                        <h4 class="mb-3">Generating PDF</h4>
                        <div class="progress" style="height: 10px;">
                            <div class="progress-bar bg-primary" 
                                 role="progressbar" 
                                 :style="{ width: progress + '%' }" 
                                 :aria-valuenow="progress" 
                                 aria-valuemin="0" 
                                 aria-valuemax="100">
                            </div>
                        </div>
                        <p class="text-muted mt-2">{{ progress }}% Complete</p>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import AllDetails from "./AllDetails.vue"
import VueHtml2pdf from "vue-html2pdf"

export default {
    name: "GeneratePdf",
    components: {
        AllDetails,
        VueHtml2pdf
    },
    data() {
        return {
            isGenerating: false,
            showProgress: false,
            progress: 0
        }
    },
    methods: {
        onProgress(event) {
            this.progress = event
            console.log(`Processed: ${event} / 100`)
        },
        onGenerated() {
            this.isGenerating = false
            this.showProgress = false
            this.progress = 0
            this.$nextTick(() => {
                this.$bvToast.toast('PDF generated successfully!', {
                    title: 'Success',
                    variant: 'success',
                    solid: true
                })
            })
        },
        generatePDF() {
            this.isGenerating = true
            this.showProgress = true
            this.progress = 0
            this.$refs.html2Pdf.generatePdf()
        }
    }
}
</script>

<style scoped>
.pdf-generator {
    background-color: var(--bg-light);
    min-height: 100vh;
    font-family: 'Inter', sans-serif;
}

.preview-wrapper {
    max-width: 1000px;
    margin: 0 auto;
}

.card {
    border-radius: 1rem;
    overflow: hidden;
    transition: transform 0.3s ease;
}

.card:hover {
    transform: translateY(-5px);
}

.card-header {
    background: linear-gradient(45deg, var(--primary-color), var(--secondary-color));
}

.icon-circle {
    width: 48px;
    height: 48px;
    background-color: rgba(255, 255, 255, 0.2);
    border-radius: 50%;
    display: flex;
    align-items: center;
    justify-content: center;
}

.btn {
    padding: 1rem 2rem;
    font-weight: 500;
    border-radius: 0.5rem;
    transition: all 0.3s ease;
}

.btn:hover:not(:disabled) {
    transform: translateY(-2px);
}

.btn-primary {
    background: linear-gradient(45deg, var(--primary-color), var(--secondary-color));
    border: none;
}

.btn:disabled {
    opacity: 0.7;
    cursor: not-allowed;
}

/* Modal Styles */
.modal {
    display: none;
    position: fixed;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    background: rgba(0, 0, 0, 0.5);
    z-index: 1050;
}

.modal.show {
    display: block;
}

.modal-dialog {
    transform: translateY(-50px);
    transition: transform 0.3s ease-out;
}

.modal.show .modal-dialog {
    transform: translateY(0);
}

.progress {
    background-color: rgba(37, 99, 235, 0.1);
    border-radius: 1rem;
    overflow: hidden;
}

.progress-bar {
    background: linear-gradient(45deg, var(--primary-color), var(--secondary-color));
    transition: width 0.6s ease;
}

.progress-icon {
    color: var(--primary-color);
}

@media (max-width: 768px) {
    .preview-wrapper {
        margin: 0 1rem;
    }

    .card-header {
        padding: 1.5rem;
    }

    .icon-circle {
        width: 40px;
        height: 40px;
    }

    .btn {
        width: 100%;
    }
}
</style>