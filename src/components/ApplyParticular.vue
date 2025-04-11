<template>
    <div class="apply-particular">
        <div class="container py-5">
            <!-- Header -->
            <div class="section-header text-center mb-5">
                <h1 class="display-5 mb-3">Job Details</h1>
                <p class="lead text-muted">Review the position details and apply</p>
            </div>

            <!-- Job Details Card -->
            <div class="job-details-wrapper">
                <div class="card border-0 shadow-sm">
                    <!-- Company Header -->
                    <div class="card-header bg-primary text-white border-0 py-4">
                        <div class="d-flex align-items-center">
                            <div class="company-logo me-4">
                                <i class="fas fa-building fa-2x"></i>
                            </div>
                            <div>
                                <h2 class="h4 mb-1">{{company}}</h2>
                                <p class="mb-0 opacity-75">
                                    <i class="fas fa-user-tie me-2"></i>
                                    {{name}}
                                </p>
                            </div>
                        </div>
                    </div>

                    <div class="card-body p-4">
                        <!-- Job Title Section -->
                        <div class="job-section mb-4">
                            <div class="section-header d-flex align-items-center mb-3">
                                <i class="fas fa-briefcase text-primary me-3"></i>
                                <h3 class="h5 mb-0">Position</h3>
                            </div>
                            <p class="h4 mb-0 ps-4">{{jtitle}}</p>
                        </div>

                        <!-- Skills Section -->
                        <div class="job-section mb-4">
                            <div class="section-header d-flex align-items-center mb-3">
                                <i class="fas fa-tools text-primary me-3"></i>
                                <h3 class="h5 mb-0">Required Skills</h3>
                            </div>
                            <div class="skills-tags ps-4">
                                <span v-for="(skill, index) in skillsList" 
                                      :key="index"
                                      class="skill-tag">
                                    <i class="fas fa-check-circle me-1"></i>
                                    {{skill}}
                                </span>
                            </div>
                        </div>

                        <!-- Description Section -->
                        <div class="job-section mb-4">
                            <div class="section-header d-flex align-items-center mb-3">
                                <i class="fas fa-align-left text-primary me-3"></i>
                                <h3 class="h5 mb-0">Job Description</h3>
                            </div>
                            <p class="description-text ps-4 mb-0">{{jdesc}}</p>
                        </div>

                        <!-- Contact Section -->
                        <div class="job-section">
                            <div class="section-header d-flex align-items-center mb-3">
                                <i class="fas fa-envelope text-primary me-3"></i>
                                <h3 class="h5 mb-0">How to Apply</h3>
                            </div>
                            <div class="contact-info ps-4">
                                <p class="mb-3">Send your resume to:</p>
                                <a :href="'mailto:' + email" class="email-link">
                                    <i class="fas fa-paper-plane me-2"></i>
                                    {{email}}
                                </a>
                            </div>
                        </div>
                    </div>
                </div>

                <!-- Action Buttons -->
                <div class="actions mt-4 d-flex justify-content-center gap-3">
                    <router-link 
                        :to="{path: '/applyjobs', query: {sid: $route.query.sid, sname: $route.query.sname}}" 
                        class="btn btn-outline-primary btn-lg">
                        <i class="fas fa-arrow-left me-2"></i>
                        Back to Jobs
                    </router-link>
                    <a :href="'mailto:' + email" class="btn btn-primary btn-lg">
                        <i class="fas fa-paper-plane me-2"></i>
                        Apply Now
                    </a>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import axios from 'axios'

export default {
    name: "ApplyParticular",
    data() {
        return {
            data: [],
            rdata: [],
            jtitle: "",
            skills: "",
            jdesc: "",
            company: "",
            name: "",
            email: "",
            rid: this.$route.query.rid,
            jid: this.$route.query.jid
        }
    },
    computed: {
        skillsList() {
            return this.skills ? this.skills.split(',').map(s => s.trim()).filter(s => s) : []
        }
    },
    beforeMount() {
        Promise.all([
            axios.get('https://resumehelpbackend.onrender.com/recruiters/jobdetget'),
            axios.get('https://resumehelpbackend.onrender.com/recruiters/get')
        ])
        .then(([jobsResponse, recruitersResponse]) => {
            this.formatPosts(jobsResponse.data)
            this.formatPosts1(recruitersResponse.data)
            this.checksid()
            this.checksid1()
        })
        .catch(error => {
            console.error('Error fetching data:', error)
        })
    },
    methods: {
        formatPosts(jobdetdata) {
            this.data = Object.keys(jobdetdata).map(key => ({
                ...jobdetdata[key],
                id: key
            }))
        },
        formatPosts1(jobdetdata) {
            this.rdata = Object.keys(jobdetdata).map(key => ({
                ...jobdetdata[key],
                id: key
            }))
        },
        checksid() {
            const job = this.data.find(item => item._id === this.jid)
            if (job) {
                this.jtitle = job.jtitle
                this.skills = job.skills
                this.jdesc = job.jdesc
            }
        },
        checksid1() {
            const recruiter = this.rdata.find(item => item._id === this.rid)
            if (recruiter) {
                this.company = recruiter.company
                this.name = recruiter.name
                this.email = recruiter.email
            }
        }
    }
}
</script>

<style scoped>
.apply-particular {
    background-color: var(--bg-light);
    min-height: 100vh;
}

.job-details-wrapper {
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

.company-logo {
    width: 48px;
    height: 48px;
    background-color: rgba(255, 255, 255, 0.2);
    border-radius: 50%;
    display: flex;
    align-items: center;
    justify-content: center;
}

.job-section {
    position: relative;
    padding-bottom: 1.5rem;
    border-bottom: 1px solid rgba(0, 0, 0, 0.1);
}

.job-section:last-child {
    padding-bottom: 0;
    border-bottom: none;
}

.section-header {
    color: var(--text-primary);
}

.skills-tags {
    display: flex;
    flex-wrap: wrap;
    gap: 0.5rem;
}

.skill-tag {
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

.email-link {
    display: inline-flex;
    align-items: center;
    color: var(--primary-color);
    font-weight: 500;
    text-decoration: none;
    padding: 0.5rem 1rem;
    background-color: rgba(37, 99, 235, 0.1);
    border-radius: 0.5rem;
    transition: all 0.3s ease;
}

.email-link:hover {
    background-color: rgba(37, 99, 235, 0.15);
    transform: translateY(-2px);
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
    .job-details-wrapper {
        margin: 0 1rem;
    }

    .card-header {
        padding: 1.5rem;
    }

    .company-logo {
        width: 40px;
        height: 40px;
    }

    .section-header h3 {
        font-size: 1.125rem;
    }

    .skill-tag {
        font-size: 0.8125rem;
    }
}
</style>