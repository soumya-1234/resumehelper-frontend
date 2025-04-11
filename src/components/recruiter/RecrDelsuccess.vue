<template>
    <div class="delete-success">
        <div class="success-overlay">
            <div class="success-content text-center">
                <div class="success-icon mb-4">
                    <i class="fas fa-check-circle"></i>
                </div>
                <h2 class="mb-4">Job Deleted Successfully</h2>
                <p class="text-muted mb-4">The job posting has been removed from your listings</p>
                <router-link 
                    :to="{path: '/recrhome', query: {rname: this.$route.query.rname, rid: this.$route.query.rid}}"
                    class="btn btn-primary">
                    <i class="fas fa-arrow-left me-2"></i>
                    Back to Dashboard
                </router-link>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    name: "RecrDelsuccess"
}
</script>

<style scoped>
.delete-success {
    min-height: 100vh;
    font-family: 'Inter', sans-serif;
}

.success-overlay {
    position: fixed;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    background: linear-gradient(rgba(0, 0, 0, 0.8), rgba(0, 0, 0, 0.9));
    display: flex;
    align-items: center;
    justify-content: center;
    padding: 2rem;
}

.success-content {
    background: white;
    padding: 3rem;
    border-radius: 1rem;
    max-width: 500px;
    width: 100%;
    animation: slideIn 0.5s ease-out;
}

.success-icon {
    font-size: 4rem;
    color: var(--success-color);
    animation: scaleIn 0.5s ease-out;
}

.success-icon i {
    animation: pulse 2s infinite;
}

h2 {
    color: var(--text-primary);
    font-weight: 600;
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

.btn-primary {
    background: linear-gradient(45deg, var(--primary-color), var(--secondary-color));
    border: none;
}

@keyframes slideIn {
    from {
        opacity: 0;
        transform: translateY(-20px);
    }
    to {
        opacity: 1;
        transform: translateY(0);
    }
}

@keyframes scaleIn {
    from {
        transform: scale(0);
    }
    to {
        transform: scale(1);
    }
}

@keyframes pulse {
    0% {
        transform: scale(1);
    }
    50% {
        transform: scale(1.1);
    }
    100% {
        transform: scale(1);
    }
}

@media (max-width: 768px) {
    .success-content {
        padding: 2rem;
        margin: 1rem;
    }

    .success-icon {
        font-size: 3rem;
    }

    h2 {
        font-size: 1.5rem;
    }
}
</style>