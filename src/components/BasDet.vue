<template>
    <div class="basic-details">
        <div class="container py-5">
            <!-- Header -->
            <div class="section-header text-center mb-5">
                <h1 class="display-5 mb-3">Basic Details</h1>
                <p class="lead text-muted">Your personal information and contact details</p>
            </div>

            <div class="details-wrapper">
                <div class="card border-0 shadow-sm">
                    <!-- Profile Header -->
                    <div class="card-header bg-primary text-white border-0 py-4">
                        <div class="d-flex align-items-center">
                            <div class="profile-avatar me-4">
                                <i class="fas fa-user fa-2x"></i>
                            </div>
                            <div>
                                <h2 class="h4 mb-1">{{$route.query.sname}}</h2>
                                <p class="mb-0 opacity-75">
                                    <i class="fas fa-id-card me-2"></i>
                                    Profile Overview
                                </p>
                            </div>
                        </div>
                    </div>

                    <div class="card-body p-4">
                        <!-- Contact Information -->
                        <div class="info-section">
                            <!-- Website -->
                            <div class="info-item mb-4">
                                <div class="info-header d-flex align-items-center mb-3">
                                    <div class="icon-circle me-3">
                                        <i class="fas fa-globe text-primary"></i>
                                    </div>
                                    <div>
                                        <h3 class="h6 text-muted mb-1">Website</h3>
                                        <a :href="website" target="_blank" class="info-link">
                                            {{website || 'Not provided'}}
                                        </a>
                                    </div>
                                </div>
                            </div>

                            <!-- Address -->
                            <div class="info-item mb-4">
                                <div class="info-header d-flex align-items-center mb-3">
                                    <div class="icon-circle me-3">
                                        <i class="fas fa-map-marker-alt text-primary"></i>
                                    </div>
                                    <div>
                                        <h3 class="h6 text-muted mb-1">Address</h3>
                                        <p class="info-text mb-0">{{address || 'Not provided'}}</p>
                                    </div>
                                </div>
                            </div>

                            <!-- Mobile -->
                            <div class="info-item">
                                <div class="info-header d-flex align-items-center mb-3">
                                    <div class="icon-circle me-3">
                                        <i class="fas fa-phone-alt text-primary"></i>
                                    </div>
                                    <div>
                                        <h3 class="h6 text-muted mb-1">Mobile Number</h3>
                                        <a :href="'tel:' + mobile" class="info-link">
                                            {{mobile || 'Not provided'}}
                                        </a>
                                    </div>
                                </div>
                            </div>
                        </div>

                        <!-- Edit Button -->
                        <div class="mt-4 text-center">
                            <router-link 
                                :to="{path: '/basicdetails', query: {sid: ssid, sname: $route.query.sname}}"
                                class="btn btn-primary">
                                <i class="fas fa-edit me-2"></i>
                                Edit Details
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
    name: "BasDet",
    data() {
        return {
            data: [],
            ssid: this.$route.query.sid,
            website: "",
            mobile: "",
            address: ""
        }
    },
    beforeMount() {
        axios.get('https://resumehelpbackend.onrender.com/students/basicdetget')
            .then((response) => {
                this.formatPosts1(response.data)
                this.checksid()
            })
            .catch(error => {
                console.error('Error fetching basic details:', error)
            })
    },
    methods: {
        formatPosts1(postData) {
            this.data = Object.keys(postData).map(key => ({
                ...postData[key],
                id: key
            }))
        },
        checksid() {
            const student = this.data.find(item => item.sid === this.ssid)
            if (student) {
                this.website = student.website
                this.mobile = student.mobile
                this.address = student.address
            }
        }
    }
}
</script>

<style scoped>
.basic-details {
    background-color: var(--bg-light);
    min-height: 100vh;
    font-family: 'Inter', sans-serif;
}

.details-wrapper {
    max-width: 700px;
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

.info-link {
    color: var(--primary-color);
    text-decoration: none;
    font-weight: 500;
    transition: color 0.3s ease;
}

.info-link:hover {
    color: var(--secondary-color);
}

.info-text {
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
}
</style>