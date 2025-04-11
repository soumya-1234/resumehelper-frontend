<template>
  <div id="app">
    <nav class="navbar navbar-expand-lg fixed-top" :class="{'navbar-light bg-light': !isDarkMode, 'navbar-dark bg-dark': isDarkMode}">
      <div class="container">
        <router-link class="logo me-auto" :to="{path: '/', query:{navb:true}}">
          <div class="logo-wrapper">
            <img src="./assets/logo.svg" alt="ResumeHelper Logo" height="40">
          </div>
        </router-link>
        <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarSupportedContent">
          <span class="navbar-toggler-icon"></span>
        </button>
        
        <!-- Navigation for public users -->
        <div class="collapse navbar-collapse" id="navbarSupportedContent" v-if="$store.state.navbar">
          <ul class="navbar-nav me-auto mb-2 mb-lg-0">
            <li class="nav-item">
              <router-link class="nav-link" :to="{path: '/products', query:{navb:true}}">
                <i class="fas fa-briefcase"></i> Services
              </router-link>
            </li>
            <li class="nav-item">
              <router-link class="nav-link" :to="{path: '/contact', query:{navb:true}}">
                <i class="fas fa-envelope"></i> Contact
              </router-link>
            </li>
          </ul>
          <div class="d-flex align-items-center">
            <button class="btn btn-theme me-2" @click="toggleTheme">
              <i :class="isDarkMode ? 'fas fa-sun' : 'fas fa-moon'"></i>
            </button>
            <button class="btn btn-primary" @click.prevent="login">
              <i class="fas fa-sign-in-alt"></i> Login
            </button>
          </div>
        </div>

        <!-- Navigation for students -->
        <div class="collapse navbar-collapse" id="navbarSupportedContent" v-if="$store.state.navbar1">
          <ul class="navbar-nav me-auto mb-2 mb-lg-0">
            <li class="nav-item">
              <router-link class="nav-link" :to="{path: '/createresume', query:{sname:this.$route.query.sname,sid:this.$route.query.sid}}">
                <i class="fas fa-file-alt"></i> Create Resume
              </router-link>
            </li>
            <li class="nav-item">
              <router-link class="nav-link" :to="{path: '/applyjobs', query:{sname:this.$route.query.sname,sid:this.$route.query.sid}}">
                <i class="fas fa-search"></i> Apply Jobs
              </router-link>
            </li>
          </ul>
          <div class="d-flex align-items-center">
            <button class="btn btn-theme me-2" @click="toggleTheme">
              <i :class="isDarkMode ? 'fas fa-sun' : 'fas fa-moon'"></i>
            </button>
            <button class="btn btn-danger" @click.prevent="logout">
              <i class="fas fa-sign-out-alt"></i> Logout
            </button>
          </div>
        </div>

        <!-- Navigation for recruiters -->
        <div class="collapse navbar-collapse" id="navbarSupportedContent" v-if="$store.state.navbar2">
          <ul class="navbar-nav me-auto mb-2 mb-lg-0">
            <li class="nav-item">
              <router-link class="nav-link" :to="{path: '/createjob', query:{rname:this.$route.query.rname,rid:this.$route.query.rid}}">
                <i class="fas fa-plus-circle"></i> Create Job
              </router-link>
            </li>
          </ul>
          <div class="d-flex align-items-center">
            <button class="btn btn-theme me-2" @click="toggleTheme">
              <i :class="isDarkMode ? 'fas fa-sun' : 'fas fa-moon'"></i>
            </button>
            <button class="btn btn-danger" @click.prevent="logoutrecr">
              <i class="fas fa-sign-out-alt"></i> Logout
            </button>
          </div>
        </div>
      </div>
    </nav>

    <div class="content-wrapper" :class="{'dark-mode': isDarkMode}">
      <router-view></router-view>
    </div>
  </div>
</template>

<script>
export default {
  name: 'App',
  data() {
    return {
      navbar: true,
      isDarkMode: false
    }
  },
  beforeUpdate() {
    this.navbar = this.$route.query.navb
  },
  methods: {
    login() {
      this.$router.push({path:"/login", query:{navb:true}})
    },
    logout() {
      this.$store.commit("setNavbar", true)
      this.$router.push({path:"/", query:{navb:true}})
    },
    logoutrecr() {
      this.$store.commit("setNavbar", true)
      this.$router.push({path:"/", query:{navb:true}})
    },
    toggleTheme() {
      this.isDarkMode = !this.isDarkMode
      document.body.classList.toggle('dark-mode')
    }
  }
}
</script>

<style>
:root {
  --primary-color: #2563eb;
  --secondary-color: #3b82f6;
  --success-color: #22c55e;
  --danger-color: #ef4444;
  --text-primary: #1f2937;
  --text-secondary: #4b5563;
  --bg-light: #f8fafc;
  --bg-dark: #1e293b;
  --transition: all 0.3s ease;
}

body {
  margin: 0;
  font-family: 'Inter', -apple-system, BlinkMacSystemFont, sans-serif;
  -webkit-font-smoothing: antialiased;
}

#app {
  min-height: 100vh;
}

.navbar {
  padding: 1rem 0;
  box-shadow: 0 2px 4px rgba(0,0,0,0.1);
  transition: var(--transition);
}

.navbar-light {
  background-color: rgba(255, 255, 255, 0.95) !important;
  backdrop-filter: blur(10px);
}

.navbar-dark {
  background-color: rgba(30, 41, 59, 0.95) !important;
  backdrop-filter: blur(10px);
}

.logo {
  display: flex;
  align-items: center;
}

.logo-wrapper {
  display: flex;
  align-items: center;
  justify-content: center;
  width: 157px;
  height: 36px;
}

.logo-wrapper img {
  object-fit: contain;
  transition: var(--transition);
}

.nav-link {
  font-weight: 500;
  padding: 0.5rem 1rem;
  transition: var(--transition);
  border-radius: 0.5rem;
  margin: 0 0.25rem;
}

.nav-link:hover {
  background-color: rgba(59, 130, 246, 0.1);
  color: var(--primary-color);
}

.navbar-dark .nav-link:hover {
  background-color: rgba(59, 130, 246, 0.2);
  color: #60a5fa;
}

.btn {
  padding: 0.5rem 1.25rem;
  font-weight: 500;
  border-radius: 0.5rem;
  transition: var(--transition);
}

.btn-primary {
  background-color: var(--primary-color);
  border-color: var(--primary-color);
}

.btn-primary:hover {
  background-color: var(--secondary-color);
  border-color: var(--secondary-color);
}

.btn-danger {
  background-color: var(--danger-color);
  border-color: var(--danger-color);
}

.btn-theme {
  background-color: transparent;
  border: 1px solid #e2e8f0;
  color: var(--text-secondary);
}

.navbar-dark .btn-theme {
  border-color: #475569;
  color: #e2e8f0;
}

.content-wrapper {
  padding-top: 5rem;
  min-height: calc(100vh - 5rem);
  background-color: var(--bg-light);
  transition: var(--transition);
}

.content-wrapper.dark-mode {
  background-color: var(--bg-dark);
  color: #e2e8f0;
}

.dark-mode {
  background-color: var(--bg-dark);
  color: #e2e8f0;
}

@media (max-width: 768px) {
  .navbar {
    padding: 0.5rem 0;
  }
  
  .logo-wrapper {
    width: 130px;
    height: 30px;
  }
  
  .btn {
    padding: 0.4rem 1rem;
    font-size: 0.9rem;
  }
}
</style>
