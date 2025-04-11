<template>
    <div class="student-home">
        <div class="container py-5">
            <!-- Profile Header -->
            <div class="profile-header text-center mb-5">
                <div class="avatar-wrapper mb-3">
                    <div class="avatar">
                        <i class="fas fa-user fa-3x"></i>
                    </div>
                </div>
                <h1 class="display-5 mb-2">{{$route.query.sname}}</h1>
                <p class="lead text-muted">Welcome to your Resume Dashboard</p>
            </div>

            <!-- Dashboard Grid -->
            <div class="dashboard-grid">
                <!-- Basic Details Card -->
                <div class="dashboard-card">
                    <div class="card-icon bg-primary-light">
                        <i class="fas fa-user-circle"></i>
                    </div>
                    <h2>Basic Details</h2>
                    <p>View and manage your personal information and contact details</p>
                    <router-link 
                        :to="{path:'/basdet',query:{sid:this.$route.query.sid,sname:this.$route.query.sname}}" 
                        class="btn btn-primary btn-lg">
                        View Details
                    </router-link>
                </div>

                <!-- Academic Details Card -->
                <div class="dashboard-card">
                    <div class="card-icon bg-success-light">
                        <i class="fas fa-graduation-cap"></i>
                    </div>
                    <h2>Academic Details</h2>
                    <p>Manage your educational background and academic achievements</p>
                    <router-link 
                        :to="{path:'/acadet',query:{sid:this.$route.query.sid,sname:this.$route.query.sname}}" 
                        class="btn btn-success btn-lg">
                        View Details
                    </router-link>
                </div>

                <!-- Skills Details Card -->
                <div class="dashboard-card">
                    <div class="card-icon bg-info-light">
                        <i class="fas fa-tools"></i>
                    </div>
                    <h2>Skills</h2>
                    <p>Add and update your technical and professional skills</p>
                    <router-link 
                        :to="{path:'/skidet',query:{sid:this.$route.query.sid,sname:this.$route.query.sname}}" 
                        class="btn btn-info btn-lg">
                        View Details
                    </router-link>
                </div>

                <!-- Project Details Card -->
                <div class="dashboard-card">
                    <div class="card-icon bg-warning-light">
                        <i class="fas fa-project-diagram"></i>
                    </div>
                    <h2>Projects</h2>
                    <p>Showcase your projects and professional experience</p>
                    <router-link 
                        :to="{path:'/prodet',query:{sid:this.$route.query.sid,sname:this.$route.query.sname}}" 
                        class="btn btn-warning btn-lg">
                        View Details
                    </router-link>
                </div>
            </div>

            <!-- Generate Resume Button -->
            <div class="text-center mt-5">
                <router-link 
                    :to="{path:'/gpdf',query:{sid:this.$route.query.sid,sname:this.$route.query.sname}}" 
                    class="btn btn-primary btn-xl">
                    <i class="fas fa-file-pdf me-2"></i>Generate Complete Resume
                </router-link>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    name: "StudentHome"
}
</script>

<style scoped>
.student-home {
    background-color: var(--bg-light);
    min-height: 100vh;
}

.profile-header {
    max-width: 600px;
    margin: 0 auto;
}

.avatar-wrapper {
    position: relative;
    display: inline-block;
}

.avatar {
    width: 100px;
    height: 100px;
    background-color: var(--primary-color);
    border-radius: 50%;
    display: flex;
    align-items: center;
    justify-content: center;
    color: white;
    box-shadow: 0 4px 6px rgba(37, 99, 235, 0.1);
}

.dashboard-grid {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
    gap: 1.5rem;
    max-width: 1200px;
    margin: 0 auto;
}

.dashboard-card {
    background: white;
    border-radius: 1rem;
    padding: 2rem;
    text-align: center;
    box-shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.1), 0 2px 4px -1px rgba(0, 0, 0, 0.06);
    transition: transform 0.3s ease;
}

.dashboard-card:hover {
    transform: translateY(-5px);
}

.card-icon {
    width: 64px;
    height: 64px;
    border-radius: 50%;
    display: flex;
    align-items: center;
    justify-content: center;
    margin: 0 auto 1.5rem;
    font-size: 1.5rem;
}

.bg-primary-light { background-color: rgba(37, 99, 235, 0.1); color: var(--primary-color); }
.bg-success-light { background-color: rgba(34, 197, 94, 0.1); color: var(--success-color); }
.bg-info-light { background-color: rgba(59, 130, 246, 0.1); color: var(--secondary-color); }
.bg-warning-light { background-color: rgba(234, 179, 8, 0.1); color: #eab308; }

.dashboard-card h2 {
    font-size: 1.25rem;
    font-weight: 600;
    margin-bottom: 1rem;
    color: var(--text-primary);
}

.dashboard-card p {
    color: var(--text-secondary);
    margin-bottom: 1.5rem;
    line-height: 1.5;
}

.btn-xl {
    padding: 1rem 2rem;
    font-size: 1.25rem;
    border-radius: 0.75rem;
    font-weight: 500;
}

@media (max-width: 768px) {
    .dashboard-grid {
        grid-template-columns: 1fr;
        padding: 0 1rem;
    }

    .dashboard-card {
        padding: 1.5rem;
    }

    .btn-xl {
        padding: 0.75rem 1.5rem;
        font-size: 1.125rem;
    }
}
</style>