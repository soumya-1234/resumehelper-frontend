<template>
    <div class="academic-details">
        <div class="container py-5">
            <!-- Header -->
            <div class="section-header text-center mb-5">
                <h1 class="display-5 mb-3">Academic Details</h1>
                <p class="lead text-muted">Your educational qualifications and achievements</p>
            </div>

            <div class="details-wrapper">
                <div class="card border-0 shadow-sm">
                    <!-- Profile Header -->
                    <div class="card-header bg-primary text-white border-0 py-4">
                        <div class="d-flex align-items-center">
                            <div class="profile-avatar me-4">
                                <i class="fas fa-user-graduate fa-2x"></i>
                            </div>
                            <div>
                                <h2 class="h4 mb-1">{{$route.query.sname}}</h2>
                                <p class="mb-0 opacity-75">
                                    <i class="fas fa-graduation-cap me-2"></i>
                                    Academic Profile
                                </p>
                            </div>
                        </div>
                    </div>

                    <div class="card-body p-4">
                        <!-- Academic Information -->
                        <div class="info-section">
                            <!-- 10th Grade -->
                            <div class="info-item mb-4">
                                <div class="info-header d-flex align-items-center mb-3">
                                    <div class="icon-circle me-3">
                                        <i class="fas fa-school text-primary"></i>
                                    </div>
                                    <div>
                                        <h3 class="h6 text-muted mb-1">10th Grade</h3>
                                        <div class="d-flex align-items-center">
                                            <div class="progress flex-grow-1 me-3" style="height: 8px;">
                                                <div class="progress-bar bg-primary" 
                                                     role="progressbar" 
                                                     :style="{ width: percentage + '%' }"
                                                     :aria-valuenow="percentage"
                                                     aria-valuemin="0"
                                                     aria-valuemax="100">
                                                </div>
                                            </div>
                                            <span class="percentage-text">{{percentage}}%</span>
                                        </div>
                                    </div>
                                </div>
                            </div>

                            <!-- 12th Grade -->
                            <div class="info-item mb-4">
                                <div class="info-header d-flex align-items-center mb-3">
                                    <div class="icon-circle me-3">
                                        <i class="fas fa-school text-primary"></i>
                                    </div>
                                    <div>
                                        <h3 class="h6 text-muted mb-1">12th Grade</h3>
                                        <div class="d-flex align-items-center">
                                            <div class="progress flex-grow-1 me-3" style="height: 8px;">
                                                <div class="progress-bar bg-primary" 
                                                     role="progressbar" 
                                                     :style="{ width: percentage1 + '%' }"
                                                     :aria-valuenow="percentage1"
                                                     aria-valuemin="0"
                                                     aria-valuemax="100">
                                                </div>
                                            </div>
                                            <span class="percentage-text">{{percentage1}}%</span>
                                        </div>
                                    </div>
                                </div>
                            </div>

                            <!-- Graduation -->
                            <div class="info-item">
                                <div class="info-header d-flex align-items-center mb-3">
                                    <div class="icon-circle me-3">
                                        <i class="fas fa-university text-primary"></i>
                                    </div>
                                    <div>
                                        <h3 class="h6 text-muted mb-1">Graduation</h3>
                                        <div class="d-flex align-items-center">
                                            <div class="progress flex-grow-1 me-3" style="height: 8px;">
                                                <div class="progress-bar bg-primary" 
                                                     role="progressbar" 
                                                     :style="{ width: percentage2 + '%' }"
                                                     :aria-valuenow="percentage2"
                                                     aria-valuemin="0"
                                                     aria-valuemax="100">
                                                </div>
                                            </div>
                                            <span class="percentage-text">{{percentage2}}%</span>
                                        </div>
                                    </div>
                                </div>
                            </div>
                        </div>

                        <!-- Edit Button -->
                        <div class="mt-4 text-center">
                            <router-link 
                                :to="{path: '/academicdetails', query: {sid: ssid, sname: $route.query.sname}}"
                                class="btn btn-primary">
                                <i class="fas fa-edit me-2"></i>
                                Edit Academic Details
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
    name: "AcaDet",
    data() {
        return {
            data: [],
            percentage: null,
            percentage1: null,
            percentage2: null,
            ssid: this.$route.query.sid
        }
    },
    beforeMount() {
        axios.get('https://resumehelpbackend.onrender.com/students/academicdetget')
            .then((response) => {
                this.formatPosts(response.data)
                this.checksid()
            })
            .catch(error => {
                console.error('Error fetching academic details:', error)
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
            const academic = this.data.find(item => item.sid === this.ssid)
            if (academic) {
                this.percentage = academic.percentage
                this.percentage1 = academic.percentage1
                this.percentage2 = academic.percentage2
            }
        }
    }
}
</script>

<style scoped>
.academic-details {
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

.progress {
    background-color: rgba(37, 99, 235, 0.1);
    border-radius: 1rem;
    overflow: hidden;
}

.progress-bar {
    transition: width 0.6s ease;
}

.percentage-text {
    color: var(--text-primary);
    font-weight: 500;
    font-size: 0.9375rem;
    min-width: 3.5rem;
    text-align: right;
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

    .percentage-text {
        font-size: 0.875rem;
    }
}
</style>