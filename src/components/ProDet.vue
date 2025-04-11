<template>
    <div class="project-details">
        <div class="container py-5">
            <!-- Header -->
            <div class="section-header text-center mb-5">
                <h1 class="display-5 mb-3">Project Details</h1>
                <p class="lead text-muted">Overview of your project information</p>
            </div>

            <div class="details-wrapper">
                <div class="card border-0 shadow-sm">
                    <!-- Project Header -->
                    <div class="card-header bg-primary text-white border-0 py-4">
                        <div class="d-flex align-items-center">
                            <div class="profile-avatar me-4">
                                <i class="fas fa-user-circle fa-2x"></i>
                            </div>
                            <div>
                                <h2 class="h4 mb-1">{{$route.query.sname}}</h2>
                                <p class="mb-0 opacity-75">
                                    <i class="fas fa-project-diagram me-2"></i>
                                    Project Overview
                                </p>
                            </div>
                        </div>
                    </div>

                    <div class="card-body p-4">
                        <!-- Project Information -->
                        <div class="info-section">
                            <!-- Project Name -->
                            <div class="info-item mb-4">
                                <div class="info-header d-flex align-items-center mb-3">
                                    <div class="icon-circle me-3">
                                        <i class="fas fa-bookmark text-primary"></i>
                                    </div>
                                    <div>
                                        <h3 class="h6 text-muted mb-1">Project Name</h3>
                                        <p class="info-text mb-0">{{projname || 'Not specified'}}</p>
                                    </div>
                                </div>
                            </div>

                            <!-- Technologies -->
                            <div class="info-item mb-4">
                                <div class="info-header d-flex align-items-center mb-3">
                                    <div class="icon-circle me-3">
                                        <i class="fas fa-code text-primary"></i>
                                    </div>
                                    <div>
                                        <h3 class="h6 text-muted mb-1">Technologies Used</h3>
                                    </div>
                                </div>
                                <div class="tech-tags ps-4">
                                    <span v-for="(tech, index) in techList" 
                                          :key="index"
                                          class="tech-tag">
                                        <i class="fas fa-check-circle me-1"></i>
                                        {{tech}}
                                    </span>
                                </div>
                            </div>

                            <!-- Description -->
                            <div class="info-item">
                                <div class="info-header d-flex align-items-center mb-3">
                                    <div class="icon-circle me-3">
                                        <i class="fas fa-align-left text-primary"></i>
                                    </div>
                                    <div>
                                        <h3 class="h6 text-muted mb-1">Project Description</h3>
                                    </div>
                                </div>
                                <div class="description ps-4">
                                    <p class="description-text mb-0">{{projdetails || 'No description provided'}}</p>
                                </div>
                            </div>
                        </div>

                        <!-- Edit Button -->
                        <div class="mt-4 text-center">
                            <router-link 
                                :to="{path: '/projectdetails', query: {sid: ssid, sname: $route.query.sname}}"
                                class="btn btn-primary">
                                <i class="fas fa-edit me-2"></i>
                                Edit Project Details
                            </router-link>
                        </div>
                    </div>
                </div>

                <!-- Back Button -->
                <div class="mt-4 text-center">
                    <router-link 
                        :to="{path: '/studenthome', query: {sid: ssid, sname: $route.query.sname}}"
                        class="btn btn-outline-primary">
                        <i class="fas fa-arrow-left me-2"></i>
                        Back to Dashboard
                    </router-link>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import axios from 'axios'

export default {
    name: "ProDet",
    data() {
        return {
            data: [],
            projname: "",
            techno: "",
            projdetails: "",
            ssid: this.$route.query.sid
        }
    },
    computed: {
        techList() {
            return this.techno ? this.techno.split(',').map(t => t.trim()).filter(t => t) : []
        }
    },
    beforeMount() {
        axios.get('https://resumehelpbackend.onrender.com/students/projectdetget')
            .then((response) => {
                this.formatPosts(response.data)
                this.checksid()
            })
            .catch(error => {
                console.error('Error fetching project details:', error)
            })
    },
    methods: {
        formatPosts(postData) {
            this.data = Object.keys(postData).map(key => ({
                ...postData[key],
                id: key
            }))
        },
        checksid() {
            const project = this.data.find(item => item.sid === this.ssid)
            if (project) {
                this.projname = project.projname
                this.techno = project.techno
                this.projdetails = project.projdetails
            }
        }
    }
}
</script>

<style scoped>
.project-details {
    background-color: var(--bg-light);
    min-height: 100vh;
    font-family: 'Inter', sans-serif;
}

.details-wrapper {
    max-width: 800px;
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

.profile-avatar {
    width: 48px;
    height: 48px;
    background-color: rgba(255, 255, 255, 0.2);
    border-radius: 50%;
    display: flex;
    align-items: center;
    justify-content: center;
}

.icon-circle {
    width: 40px;
    height: 40px;
    background-color: rgba(37, 99, 235, 0.1);
    border-radius: 50%;
    display: flex;
    align-items: center;
    justify-content: center;
}

.info-section {
    position: relative;
}

.info-item {
    padding-bottom: 1.5rem;
    border-bottom: 1px solid rgba(0, 0, 0, 0.1);
}

.info-item:last-child {
    padding-bottom: 0;
    border-bottom: none;
}

.info-text {
    color: var(--text-primary);
    font-size: 1.125rem;
    font-weight: 500;
}

.tech-tags {
    display: flex;
    flex-wrap: wrap;
    gap: 0.5rem;
}

.tech-tag {
    background-color: rgba(37, 99, 235, 0.1);
    color: var(--primary-color);
    padding: 0.5rem 1rem;
    border-radius: 2rem;
    font-size: 0.875rem;
    font-weight: 500;
}

.description-text {
    color: var(--text-secondary);
    font-size: 0.9375rem;
    line-height: 1.6;
}

.btn {
    padding: 0.75rem 1.5rem;
    font-weight: 500;
    border-radius: 0.5rem;
    transition: all 0.3s ease;
}

.btn:hover {
    transform: translateY(-2px);
}

@media (max-width: 768px) {
    .details-wrapper {
        margin: 0 1rem;
    }

    .card-header {
        padding: 1.5rem;
    }

    .profile-avatar {
        width: 40px;
        height: 40px;
    }

    .icon-circle {
        width: 36px;
        height: 36px;
    }

    .info-text {
        font-size: 1rem;
    }

    .tech-tag {
        font-size: 0.8125rem;
    }
}
</style>