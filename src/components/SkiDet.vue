<template>
    <div class="skill-details">
        <div class="container py-5">
            <!-- Header -->
            <div class="section-header text-center mb-5">
                <h1 class="display-5 mb-3">Skills Overview</h1>
                <p class="lead text-muted">Your technical and professional competencies</p>
            </div>

            <!-- Skills Card -->
            <div class="skills-card">
                <div class="card border-0 shadow-sm">
                    <div class="card-header bg-primary text-white border-0 py-3">
                        <div class="d-flex align-items-center">
                            <i class="fas fa-user-circle fa-2x me-3"></i>
                            <div>
                                <h2 class="h4 mb-1">{{$route.query.sname}}</h2>
                                <p class="mb-0 opacity-75">Professional Profile</p>
                            </div>
                        </div>
                    </div>
                    <div class="card-body p-4">
                        <div class="skills-section">
                            <h3 class="h5 mb-4">
                                <i class="fas fa-tools me-2"></i>
                                Technical Skills
                            </h3>
                            <div class="skills-container" v-if="skill">
                                <div class="skills-grid">
                                    <div 
                                        v-for="(skill, index) in skillsList" 
                                        :key="index"
                                        class="skill-item"
                                    >
                                        <div class="skill-tag">
                                            <i class="fas fa-check-circle me-2"></i>
                                            {{skill}}
                                        </div>
                                    </div>
                                </div>
                            </div>
                            <div v-else class="no-skills">
                                <p class="text-muted text-center mb-0">
                                    <i class="fas fa-info-circle me-2"></i>
                                    No skills have been added yet
                                </p>
                            </div>
                        </div>
                    </div>
                </div>

                <!-- Actions -->
                <div class="actions mt-4 text-center">
                    <router-link 
                        :to="{
                            path: '/createresume',
                            query: {
                                sid: this.$route.query.sid,
                                sname: this.$route.query.sname
                            }
                        }" 
                        class="btn btn-primary btn-lg"
                    >
                        <i class="fas fa-edit me-2"></i>
                        Edit Skills
                    </router-link>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import axios from 'axios'

export default {
    name: "SkiDet",
    data() {
        return {
            data: [],
            skill: "",
            ssid: this.$route.query.sid
        }
    },
    computed: {
        skillsList() {
            return this.skill ? this.skill.split(',').map(s => s.trim()).filter(s => s) : []
        }
    },
    beforeMount() {
        axios.get('https://resumehelpbackend.onrender.com/students/skilldetget')
            .then((response) => {
                this.formatPosts(response.data)
                this.checksid()
            })
            .catch(error => {
                console.error('Error fetching skills:', error)
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
            const userSkills = this.data.find(item => item.sid === this.ssid)
            if (userSkills) {
                this.skill = userSkills.skill
            }
        }
    }
}
</script>

<style scoped>
.skill-details {
    background-color: var(--bg-light);
    min-height: 100vh;
}

.skills-card {
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

.skills-section {
    position: relative;
}

.skills-grid {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(200px, 1fr));
    gap: 1rem;
}

.skill-item {
    display: flex;
    align-items: center;
}

.skill-tag {
    background-color: rgba(37, 99, 235, 0.1);
    color: var(--primary-color);
    padding: 0.75rem 1rem;
    border-radius: 0.5rem;
    font-weight: 500;
    width: 100%;
    transition: all 0.3s ease;
}

.skill-tag:hover {
    background-color: rgba(37, 99, 235, 0.15);
    transform: translateY(-2px);
}

.no-skills {
    background-color: rgba(0, 0, 0, 0.02);
    padding: 2rem;
    border-radius: 0.5rem;
}

.btn-lg {
    padding: 0.75rem 1.5rem;
    font-weight: 500;
    border-radius: 0.5rem;
}

@media (max-width: 768px) {
    .skills-card {
        margin: 0 1rem;
    }

    .skills-grid {
        grid-template-columns: 1fr;
    }

    .card-header h2 {
        font-size: 1.25rem;
    }
}
</style>