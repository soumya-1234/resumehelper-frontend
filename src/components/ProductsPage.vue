<template>
    <div class="products-page">
        <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.2.2/dist/css/bootstrap.min.css" rel="stylesheet" integrity="sha384-Zenh87qX5JnK2Jl0vWa8Ck2rdkQ2Bzep5IDxbcnCeuOxjzrPF/et3URy9Bv1WTRi" crossorigin="anonymous">
        
        <section class="hero-section text-center py-5">
            <div class="container">
                <h1 class="display-4 mb-4">
                    <span class="text-primary">Resume</span><span class="text-danger">Helper</span>
                </h1>
                <h2 class="h3 text-secondary mb-4">Building Tools for Students and Recruiters</h2>
                <p class="lead text-muted mb-5">
                    We provide a comprehensive platform that connects talented students with top recruiters. 
                    Our tools are designed to streamline the resume creation and job application process, 
                    making it easier for students to showcase their skills and for recruiters to find the perfect candidates.
                </p>
            </div>
        </section>

        <section class="services-section py-5 bg-light">
            <div class="container">
                <h2 class="text-center mb-5">Our Services</h2>
                <div class="row g-4">
                    <div class="col-md-4">
                        <div class="card h-100 border-0 shadow-sm hover-card">
                            <div class="card-img-wrapper">
                                <img src="../assets/resumepic2.png" class="card-img-top" alt="Create Resume" height="250">
                            </div>
                            <div class="card-body text-center">
                                <div class="service-icon mb-3">
                                    <i class="fas fa-file-alt fa-2x text-primary"></i>
                                </div>
                                <h3 class="card-title h4 mb-3">Create Professional Resumes</h3>
                                <p class="card-text">Build an impressive resume with our easy-to-use tools. Choose from professional templates, customize your content, and get expert tips for making your resume stand out.</p>
                            </div>
                        </div>
                    </div>
                    <div class="col-md-4">
                        <div class="card h-100 border-0 shadow-sm hover-card">
                            <div class="card-img-wrapper">
                                <img src="../assets/resume3.png" class="card-img-top" alt="Post Jobs" height="250">
                            </div>
                            <div class="card-body text-center">
                                <div class="service-icon mb-3">
                                    <i class="fas fa-briefcase fa-2x text-primary"></i>
                                </div>
                                <h3 class="card-title h4 mb-3">Post Job Opportunities</h3>
                                <p class="card-text">Recruiters can easily post job openings, specify requirements, and reach out to qualified candidates. Our platform makes hiring efficient and effective.</p>
                            </div>
                        </div>
                    </div>
                    <div class="col-md-4">
                        <div class="card h-100 border-0 shadow-sm hover-card">
                            <div class="card-img-wrapper">
                                <img src="../assets/resume4.jpg" class="card-img-top" alt="Apply Jobs" height="250">
                            </div>
                            <div class="card-body text-center">
                                <div class="service-icon mb-3">
                                    <i class="fas fa-search fa-2x text-primary"></i>
                                </div>
                                <h3 class="card-title h4 mb-3">Apply to Jobs</h3>
                                <p class="card-text">Students can browse through job listings, filter by their preferences, and apply directly through our platform. Track your applications and get notifications for updates.</p>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </section>

        <section class="cta-section py-5">
            <div class="container text-center">
                <h2 class="mb-4">Ready to Get Started?</h2>
                <div class="d-flex justify-content-center gap-3">
                    <button class="btn btn-primary btn-lg" @click="$router.push('/signup')">
                        <i class="fas fa-user-plus me-2"></i>Sign Up Now
                    </button>
                    <button class="btn btn-outline-primary btn-lg" @click="$router.push('/contact')">
                        <i class="fas fa-envelope me-2"></i>Contact Us
                    </button>
                </div>
            </div>
        </section>
    </div>
</template>

<script>
export default {
    name: "ProductsPage"
}
</script>

<style scoped>
.products-page {
    overflow-x: hidden;
}

.hero-section {
    background: linear-gradient(135deg, var(--bg-light) 0%, #e2e8f0 100%);
}

.lead {
    max-width: 800px;
    margin: 0 auto;
}

.services-section {
    background-color: var(--bg-light);
}

.card {
    transition: transform 0.3s ease;
    border-radius: 1rem;
    overflow: hidden;
}

.card:hover {
    transform: translateY(-5px);
}

.card-img-wrapper {
    overflow: hidden;
}

.card-img-top {
    object-fit: cover;
    transition: transform 0.3s ease;
}

.card:hover .card-img-top {
    transform: scale(1.05);
}

.service-icon {
    width: 64px;
    height: 64px;
    margin: 0 auto;
    display: flex;
    align-items: center;
    justify-content: center;
    background-color: rgba(37, 99, 235, 0.1);
    border-radius: 50%;
}

.card-title {
    color: var(--text-primary);
    font-weight: 600;
}

.card-text {
    color: var(--text-secondary);
}

.cta-section {
    background: linear-gradient(135deg, var(--primary-color) 0%, var(--secondary-color) 100%);
    color: white;
}

.btn-outline-primary {
    border-color: white;
    color: white;
}

.btn-outline-primary:hover {
    background-color: white;
    color: var(--primary-color);
    border-color: white;
}

@media (max-width: 768px) {
    .card-img-top {
        height: 200px;
    }
    
    .lead {
        font-size: 1rem;
    }
}
</style>