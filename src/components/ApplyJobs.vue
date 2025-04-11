<template>
    <div class="apply-jobs">
        <div class="container py-5">
            <!-- Header -->
            <div class="section-header text-center mb-5">
                <h1 class="display-5 mb-3">Available Jobs</h1>
                <p class="lead text-muted">Explore and apply for positions that match your skills</p>
            </div>

            <!-- Jobs Grid -->
            <div class="jobs-grid">
                <div class="row g-4">
                    <div class="col-md-6 col-lg-4" v-for="job in data" :key="job._id">
                        <div class="job-card">
                            <div class="card border-0 shadow-sm h-100">
                                <div class="card-header bg-primary text-white border-0 py-3">
                                    <div class="d-flex align-items-center">
                                        <i class="fas fa-briefcase fa-2x me-3"></i>
                                        <h2 class="h5 mb-0">{{job.jtitle}}</h2>
                                    </div>
                                </div>
                                <div class="card-body p-4">
                                    <!-- Skills Section -->
                                    <div class="skills-section mb-4">
                                        <h3 class="h6 text-muted mb-3">Required Skills</h3>
                                        <div class="skills-tags">
                                            <span v-for="(skill, index) in getSkillsList(job.skills)" 
                                                  :key="index"
                                                  class="skill-tag">
                                                <i class="fas fa-check-circle me-1"></i>
                                                {{skill}}
                                            </span>
                                        </div>
                                    </div>

                                    <!-- Description -->
                                    <div class="description-section">
                                        <h3 class="h6 text-muted mb-3">Job Description</h3>
                                        <p class="description-text mb-4">{{job.jdesc}}</p>
                                    </div>

                                    <!-- Apply Button -->
                                    <button 
                                        class="btn btn-primary w-100" 
                                        @click.prevent="applyjob(job.rid, job._id)">
                                        <i class="fas fa-paper-plane me-2"></i>
                                        Apply Now
                                    </button>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
            </div>

            <!-- No Jobs Message -->
            <div v-if="data.length === 0" class="no-jobs text-center py-5">
                <div class="empty-state">
                    <i class="fas fa-briefcase fa-3x mb-3 text-muted"></i>
                    <h3 class="h4 mb-3">No Jobs Available</h3>
                    <p class="text-muted">Check back later for new opportunities</p>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import axios from 'axios'

export default {
    name: "ApplyJobs",
    data() {
        return {
            data: [],
            rrid: this.$route.query.rid
        }
    },
    beforeMount() {
        axios.get('https://resumehelpbackend.onrender.com/recruiters/jobdetget')
            .then((response) => {
                this.formatposts(response.data)
            })
            .catch(error => {
                console.error('Error fetching jobs:', error)
            })
    },
    methods: {
        formatposts(jobdetdata) {
            this.data = Object.keys(jobdetdata).map(key => ({
                ...jobdetdata[key],
                id: key
            }))
        },
        applyjob(rid, jid) {
            this.$router.push({
                path: "/applypart",
                query: {
                    navb: false,
                    sid: this.$route.query.sid,
                    sname: this.$route.query.sname,
                    rid: rid,
                    jid: jid
                }
            })
        },
        getSkillsList(skills) {
            return skills.split(',').map(s => s.trim()).filter(s => s)
        }
    }
}
</script>

<style scoped>
.apply-jobs {
    background-color: var(--bg-light);
    min-height: 100vh;
}

.job-card {
    height: 100%;
    transition: transform 0.3s ease;
}

.job-card:hover {
    transform: translateY(-5px);
}

.card {
    border-radius: 1rem;
    overflow: hidden;
}

.card-header {
    background: linear-gradient(45deg, var(--primary-color), var(--secondary-color));
}

.skills-tags {
    display: flex;
    flex-wrap: wrap;
    gap: 0.5rem;
    margin-bottom: 1rem;
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

.empty-state {
    color: var(--text-secondary);
    padding: 3rem;
    background-color: white;
    border-radius: 1rem;
    box-shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.1);
}

.btn {
    border-radius: 0.5rem;
    padding: 0.75rem 1.5rem;
    font-weight: 500;
    transition: all 0.3s ease;
}

.btn:hover {
    transform: translateY(-2px);
}

@media (max-width: 768px) {
    .section-header {
        padding: 0 1rem;
    }

    .card-header h2 {
        font-size: 1.125rem;
    }

    .skill-tag {
        font-size: 0.8125rem;
    }
}
</style>