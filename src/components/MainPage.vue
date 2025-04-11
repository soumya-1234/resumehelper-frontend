<template>
    <div class="main-page">
        <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.2.2/dist/css/bootstrap.min.css" rel="stylesheet" integrity="sha384-Zenh87qX5JnK2Jl0vWa8Ck2rdkQ2Bzep5IDxbcnCeuOxjzrPF/et3URy9Bv1WTRi" crossorigin="anonymous">
        
        <section class="hero-section">
            <div class="container">
                <div class="row align-items-center">
                    <div class="col-lg-6">
                        <h1 class="hero-title">Create Your Professional Resume</h1>
                        <p class="hero-subtitle">Stand out from the crowd with a beautifully designed resume. Easy to use, professional templates, and expert tips to help you land your dream job.</p>
                        <div class="hero-buttons">
                            <button class="btn btn-primary btn-lg me-3" @click="$router.push('/signup')">Get Started</button>
                            <button class="btn btn-outline-primary btn-lg" @click="$router.push('/products')">Learn More</button>
                        </div>
                    </div>
                    <div class="col-lg-6">
                        <div class="hero-carousel">
                            <div id="carouselExampleIndicators" class="carousel slide" data-bs-ride="true">
                                <div class="carousel-indicators">
                                    <button type="button" data-bs-target="#carouselExampleIndicators" data-bs-slide-to="0" class="active" aria-current="true" aria-label="Slide 1"></button>
                                    <button type="button" data-bs-target="#carouselExampleIndicators" data-bs-slide-to="1" aria-label="Slide 2"></button>
                                    <button type="button" data-bs-target="#carouselExampleIndicators" data-bs-slide-to="2" aria-label="Slide 3"></button>
                                </div>
                                <div class="carousel-inner rounded-4 shadow">
                                    <div class="carousel-item active">
                                        <img src="../assets/resumepic1.png" class="d-block w-100" alt="Resume Builder Preview" width="400" height="500">
                                    </div>
                                    <div class="carousel-item">
                                        <img src="../assets/resumebuilder.png" class="d-block w-100" alt="Resume Template" width="400" height="500">
                                    </div>
                                    <div class="carousel-item">
                                        <img src="../assets/resume5.jpg" class="d-block w-100" alt="Professional Resume" width="400" height="500">
                                    </div>
                                </div>
                                <button class="carousel-control-prev" type="button" data-bs-target="#carouselExampleIndicators" data-bs-slide="prev">
                                    <span class="carousel-control-prev-icon" aria-hidden="true"></span>
                                    <span class="visually-hidden">Previous</span>
                                </button>
                                <button class="carousel-control-next" type="button" data-bs-target="#carouselExampleIndicators" data-bs-slide="next">
                                    <span class="carousel-control-next-icon" aria-hidden="true"></span>
                                    <span class="visually-hidden">Next</span>
                                </button>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </section>

        <section class="stats-section py-5">
            <div class="container">
                <div class="row g-4">
                    <div class="col-sm-6">
                        <div class="card h-100 border-0 shadow-sm hover-card">
                            <div class="card-body text-center p-4">
                                <div class="stats-icon mb-3">
                                    <i class="fas fa-users fa-2x text-primary"></i>
                                </div>
                                <h2 class="card-title stats-number">{{sdata.length}}</h2>
                                <h3 class="stats-label">Students Registered</h3>
                                <p class="card-text">Join our growing community of successful job seekers who have created professional resumes and landed their dream jobs.</p>
                            </div>
                        </div>
                    </div>
                    <div class="col-sm-6">
                        <div class="card h-100 border-0 shadow-sm hover-card">
                            <div class="card-body text-center p-4">
                                <div class="stats-icon mb-3">
                                    <i class="fas fa-building fa-2x text-primary"></i>
                                </div>
                                <h2 class="card-title stats-number">{{rdata.length}}</h2>
                                <h3 class="stats-label">Active Recruiters</h3>
                                <p class="card-text">Connect with top companies and recruiters who are looking for talented professionals like you.</p>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </section>

        <section class="features-section py-5 bg-light">
            <div class="container">
                <h2 class="text-center mb-5">Why Choose ResumeHelper?</h2>
                <div class="row g-4">
                    <div class="col-md-4">
                        <div class="feature-card text-center p-4">
                            <i class="fas fa-magic fa-2x text-primary mb-3"></i>
                            <h3>Easy to Use</h3>
                            <p>Create a professional resume in minutes with our intuitive builder</p>
                        </div>
                    </div>
                    <div class="col-md-4">
                        <div class="feature-card text-center p-4">
                            <i class="fas fa-paint-brush fa-2x text-primary mb-3"></i>
                            <h3>Beautiful Templates</h3>
                            <p>Choose from our collection of ATS-friendly resume templates</p>
                        </div>
                    </div>
                    <div class="col-md-4">
                        <div class="feature-card text-center p-4">
                            <i class="fas fa-check-circle fa-2x text-primary mb-3"></i>
                            <h3>Expert Approved</h3>
                            <p>Get tips and suggestions from industry professionals</p>
                        </div>
                    </div>
                </div>
            </div>
        </section>
    </div>
</template>

<script>
    import axios from 'axios'
    export default{
        name: "MainPage",
        data(){
            return {
                sdata: [],
                rdata: []
            }
        },
        beforeMount(){
            axios.get('https://resumehelpbackend.onrender.com/students/get')
            .then((response)=>{
                this.formatPosts(response.data)
            })
            axios.get('https://resumehelpbackend.onrender.com/recruiters/get')
            .then((response)=>{
                this.formatPosts1(response.data)
            })
        },
        methods: {
            formatPosts(postData){
                for (let key in postData){
                    this.sdata.push({...postData[key],id:key});
                }
            },
            formatPosts1(postData){
                for (let key in postData){
                    this.rdata.push({...postData[key],id:key});
                }
            }
        }
    }
</script>

<style scoped>
.main-page {
    overflow-x: hidden;
}

.hero-section {
    padding: 5rem 0 3rem;
    background: linear-gradient(135deg, var(--bg-light) 0%, #e2e8f0 100%);
}

.hero-title {
    font-size: 3rem;
    font-weight: 700;
    margin-bottom: 1.5rem;
    color: var(--text-primary);
    line-height: 1.2;
}

.hero-subtitle {
    font-size: 1.25rem;
    color: var(--text-secondary);
    margin-bottom: 2rem;
    line-height: 1.6;
}

.hero-buttons {
    margin-bottom: 2rem;
}

.hero-carousel {
    position: relative;
    z-index: 1;
}

.carousel {
    border-radius: 1rem;
    overflow: hidden;
}

.carousel-item img {
    object-fit: cover;
    height: 500px;
}

.stats-section {
    background-color: white;
}

.hover-card {
    transition: transform 0.3s ease;
    border-radius: 1rem;
}

.hover-card:hover {
    transform: translateY(-5px);
}

.stats-icon {
    width: 64px;
    height: 64px;
    margin: 0 auto;
    display: flex;
    align-items: center;
    justify-content: center;
    background-color: rgba(37, 99, 235, 0.1);
    border-radius: 50%;
}

.stats-number {
    font-size: 2.5rem;
    font-weight: 700;
    color: var(--primary-color);
    margin-bottom: 0.5rem;
}

.stats-label {
    font-size: 1.25rem;
    font-weight: 600;
    color: var(--text-primary);
    margin-bottom: 1rem;
}

.features-section {
    background-color: var(--bg-light);
}

.feature-card {
    background-color: white;
    border-radius: 1rem;
    transition: transform 0.3s ease;
}

.feature-card:hover {
    transform: translateY(-5px);
}

.feature-card h3 {
    font-size: 1.25rem;
    font-weight: 600;
    color: var(--text-primary);
    margin: 1rem 0;
}

.feature-card p {
    color: var(--text-secondary);
    margin-bottom: 0;
}

@media (max-width: 992px) {
    .hero-section {
        padding: 4rem 0 2rem;
        text-align: center;
    }

    .hero-title {
        font-size: 2.5rem;
    }

    .hero-carousel {
        margin-top: 2rem;
    }

    .carousel-item img {
        height: 400px;
    }
}

@media (max-width: 768px) {
    .hero-title {
        font-size: 2rem;
    }

    .hero-subtitle {
        font-size: 1.1rem;
    }

    .carousel-item img {
        height: 300px;
    }

    .stats-number {
        font-size: 2rem;
    }

    .stats-label {
        font-size: 1.1rem;
    }
}
</style>