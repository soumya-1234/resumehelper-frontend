<template>
    <div class="create-resume">
        <div class="container py-4">
            <div class="resume-header text-center mb-5">
                <h1 class="display-5 mb-3">Create Your Resume</h1>
                <p class="lead text-muted">Fill in your details step by step to create a professional resume</p>
            </div>

            <div class="resume-steps mb-5">
                <div class="progress-track mb-4">
                    <div class="progress" style="height: 4px;">
                        <div class="progress-bar" role="progressbar" :style="progressStyle"></div>
                    </div>
                </div>

                <div class="step-buttons d-flex justify-content-center flex-wrap gap-3">
                    <button type="button" 
                        class="btn btn-lg step-btn" 
                        :class="{'btn-primary': text === 'BasicDetails', 'btn-outline-primary': text !== 'BasicDetails'}"
                        @click="text='BasicDetails'">
                        <i class="fas fa-user me-2"></i>Basic Details
                    </button>
                    <button type="button" 
                        class="btn btn-lg step-btn" 
                        :class="{'btn-primary': text === 'AcademicDetails', 'btn-outline-primary': text !== 'AcademicDetails'}"
                        @click="text='AcademicDetails'">
                        <i class="fas fa-graduation-cap me-2"></i>Academic Details
                    </button>
                    <button type="button" 
                        class="btn btn-lg step-btn" 
                        :class="{'btn-primary': text === 'SkillsDetails', 'btn-outline-primary': text !== 'SkillsDetails'}"
                        @click="text='SkillsDetails'">
                        <i class="fas fa-tools me-2"></i>Skills
                    </button>
                    <button type="button" 
                        class="btn btn-lg step-btn" 
                        :class="{'btn-primary': text === 'ProjectDetails', 'btn-outline-primary': text !== 'ProjectDetails'}"
                        @click="text='ProjectDetails'">
                        <i class="fas fa-project-diagram me-2"></i>Projects
                    </button>
                </div>
            </div>

            <div class="component-wrapper">
                <keep-alive>
                    <component :is="text" :msg="ssid" @step-completed="updateProgress"/>
                </keep-alive>
            </div>
        </div>
    </div>
</template>

<script>
import BasicDetails from "./BasicDetails.vue"
import AcademicDetails from "./AcademicDetails.vue"
import SkillsDetails from "./SkillsDetails.vue"
import ProjectDetails from "./ProjectDetails.vue"

export default {
    name: "CreateResume",
    components: {
        BasicDetails,
        AcademicDetails,
        SkillsDetails,
        ProjectDetails
    },
    data() {
        return {
            text: "BasicDetails",
            ssid: this.$route.query.sid,
            steps: {
                BasicDetails: { completed: false, order: 1 },
                AcademicDetails: { completed: false, order: 2 },
                SkillsDetails: { completed: false, order: 3 },
                ProjectDetails: { completed: false, order: 4 }
            }
        }
    },
    computed: {
        progressStyle() {
            const completedSteps = Object.values(this.steps).filter(step => step.completed).length;
            const progress = (completedSteps / 4) * 100;
            return `width: ${progress}%`;
        }
    },
    methods: {
        updateProgress(stepName, completed) {
            if (this.steps[stepName]) {
                this.steps[stepName].completed = completed;
            }
        }
    }
}
</script>

<style scoped>
.create-resume {
    background-color: var(--bg-light);
    min-height: 100vh;
    padding-top: 2rem;
}

.resume-header {
    max-width: 600px;
    margin: 0 auto;
}

.progress-track {
    max-width: 800px;
    margin: 0 auto;
}

.progress {
    background-color: rgba(37, 99, 235, 0.1);
    border-radius: 2px;
}

.progress-bar {
    background-color: var(--primary-color);
    transition: width 0.3s ease;
}

.step-buttons {
    position: relative;
    z-index: 1;
}

.step-btn {
    min-width: 180px;
    transition: all 0.3s ease;
    border-radius: 0.75rem;
    font-weight: 500;
}

.step-btn:hover {
    transform: translateY(-2px);
}

.component-wrapper {
    max-width: 800px;
    margin: 0 auto;
    background: white;
    border-radius: 1rem;
    box-shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.1), 0 2px 4px -1px rgba(0, 0, 0, 0.06);
    padding: 2rem;
}

@media (max-width: 768px) {
    .create-resume {
        padding-top: 1rem;
    }

    .step-btn {
        min-width: 140px;
        font-size: 0.9rem;
        padding: 0.5rem 1rem;
    }

    .component-wrapper {
        padding: 1rem;
    }
}
</style>