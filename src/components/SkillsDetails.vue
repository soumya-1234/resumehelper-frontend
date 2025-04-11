<template>
    <div class="skills-details">
        <div class="container py-5">
            <div class="section-header text-center mb-5">
                <h1 class="display-5 mb-3">Skills</h1>
                <p class="lead text-muted">Add your technical and professional skills</p>
            </div>

            <div class="form-wrapper">
                <div class="card border-0 shadow-sm">
                    <div class="card-body p-4">
                        <!-- Alerts -->
                        <transition name="fade">
                            <div class="alert alert-danger d-flex align-items-center" v-if="bool">
                                <i class="fas fa-exclamation-circle me-2"></i>
                                <div>Empty fields are not allowed</div>
                            </div>
                        </transition>
                        <transition name="fade">
                            <div class="alert alert-success d-flex align-items-center" v-if="bool1">
                                <i class="fas fa-check-circle me-2"></i>
                                <div>{{message}} Successfully</div>
                            </div>
                        </transition>

                        <!-- Skills Form -->
                        <form @submit.prevent="bool2 ? askills : askillsupdate">
                            <div class="form-group mb-4">
                                <label class="form-label h6 mb-3">Your Skills</label>
                                <div class="input-group">
                                    <span class="input-group-text bg-light border-0">
                                        <i class="fas fa-tools"></i>
                                    </span>
                                    <input 
                                        type="text" 
                                        class="form-control form-control-lg border-0 bg-light" 
                                        placeholder="Enter your skills (e.g. JavaScript, React, Node.js)"
                                        v-model.trim="formdata.skill"
                                    />
                                </div>
                                <small class="text-muted mt-2">
                                    Separate multiple skills with commas
                                </small>
                            </div>

                            <!-- Action Buttons -->
                            <div class="d-grid gap-2">
                                <button 
                                    type="submit"
                                    class="btn btn-lg"
                                    :class="bool2 ? 'btn-primary' : 'btn-warning'"
                                >
                                    <i :class="bool2 ? 'fas fa-plus me-2' : 'fas fa-edit me-2'"></i>
                                    {{ bool2 ? 'Add Skills' : 'Update Skills' }}
                                </button>
                            </div>
                        </form>

                        <!-- Skills Preview -->
                        <div class="skills-preview mt-4" v-if="formdata.skill">
                            <h6 class="mb-3">Skills Preview</h6>
                            <div class="skills-tags">
                                <span 
                                    v-for="(skill, index) in skillsList" 
                                    :key="index"
                                    class="skill-tag"
                                >
                                    {{skill}}
                                </span>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import axios from "axios"

export default {
    name: "SkillsDetails",
    data() {
        return {
            formdata: {
                sid: "",
                skill: ""
            },
            bool: false,
            bool1: false,
            bool2: true,
            ssid: this.$route.query.sid,
            skid: "",
            message: "",
            data: []
        }
    },
    computed: {
        skillsList() {
            return this.formdata.skill.split(',').map(s => s.trim()).filter(s => s)
        }
    },
    props: {
        msg: String
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
        async askills() {
            this.formdata.sid = this.msg
            if (!this.formdata.skill) {
                this.bool = true
                this.bool1 = false
                return
            }

            try {
                await axios.post("https://resumehelpbackend.onrender.com/students/skilldetcreate", this.formdata);
                this.bool = false
                this.bool1 = true
                this.message = "Skills Added"
                setTimeout(() => {
                    this.bool1 = false
                }, 3000)
            } catch (error) {
                console.error('Error adding skills:', error)
            }
        },
        formatPosts(postData) {
            this.data = Object.keys(postData).map(key => ({
                ...postData[key],
                id: key
            }))
        },
        checksid() {
            const userSkills = this.data.find(item => item.sid === this.ssid)
            if (userSkills) {
                this.bool2 = false
                this.formdata.skill = userSkills.skill
                this.skid = userSkills._id
            }
        },
        async askillsupdate() {
            if (!this.formdata.skill) {
                this.bool = true
                this.bool1 = false
                return
            }

            const updatedData = {
                sid: this.ssid,
                skill: this.formdata.skill
            }

            try {
                await axios.put(`https://resumehelpbackend.onrender.com/students/skilldetupdate/${this.skid}`, updatedData);
                this.bool = false
                this.bool1 = true
                this.message = "Skills Updated"
                setTimeout(() => {
                    this.bool1 = false
                }, 3000)
            } catch (error) {
                console.error('Error updating skills:', error)
            }
        }
    }
}
</script>

<style scoped>
.skills-details {
    background-color: var(--bg-light);
    min-height: 100vh;
}

.form-wrapper {
    max-width: 600px;
    margin: 0 auto;
}

.card {
    border-radius: 1rem;
    transition: transform 0.3s ease;
}

.form-control {
    border-radius: 0.5rem;
    padding: 0.75rem 1rem;
}

.form-control:focus {
    box-shadow: none;
    border-color: var(--primary-color);
}

.input-group-text {
    border-radius: 0.5rem 0 0 0.5rem;
    padding: 0.75rem 1rem;
    color: var(--text-secondary);
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

.fade-enter-active, .fade-leave-active {
    transition: opacity 0.3s ease;
}

.fade-enter-from, .fade-leave-to {
    opacity: 0;
}

@media (max-width: 768px) {
    .form-wrapper {
        padding: 0 1rem;
    }

    .card {
        border-radius: 0.75rem;
    }

    .form-control, .input-group-text {
        font-size: 1rem;
    }
}
</style>