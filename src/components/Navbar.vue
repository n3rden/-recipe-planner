<template>
  <div id="app" class="col-md-12">
    <div class="shadow">
      <nav class="navbar navbar-expand-lg navbar-light bg-light">
        <router-link to="/" class="navbar-brand">Recipe Planner</router-link>
        <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#nav_collapse" aria-controls="nav_collapse" aria-expanded="false" aria-label="Toggle navigation">
          <span class="navbar-toggler-icon"></span>
        </button>

        <div class="collapse navbar-collapse" id="nav_collapse">
          <template v-if="user.loggedIn">
            <ul class="navbar-nav me-auto">
              <li class="nav-item">
                <router-link to="/" class="nav-link">Create Menu</router-link>
              </li>
              <li class="nav-item">
                <router-link to="/addRecipe" class="nav-link">Add Recipe</router-link>
              </li>
              <li class="nav-item">
                <router-link to="/manageRecipes" class="nav-link">Manage Recipes</router-link>
              </li>
            </ul>

            <ul class="navbar-nav ms-auto">
              <li class="nav-item dropdown">
                <a class="nav-link dropdown-toggle" href="#" id="navbarDropdown" role="button" data-bs-toggle="dropdown" aria-expanded="false">
                  <i class="bi bi-list"></i>
                </a>
                <ul class="dropdown-menu dropdown-menu-end" aria-labelledby="navbarDropdown">
                  <li>
                    <router-link to="Profile" class="dropdown-item"><i class="bi bi-person"></i> {{user.data.displayName}}</router-link>
                  </li>
                  <li>
                    <a class="dropdown-item" @click="signOut">Sign out</a>
                  </li>
                </ul>
              </li>
            </ul>
          </template>
          <template v-else>
            <ul class="navbar-nav ms-auto">
              <li class="nav-item">
                <router-link to="Login" class="nav-link">Login</router-link>
              </li>
              <li class="nav-item">
                <router-link to="Signup" class="nav-link">Register</router-link>
              </li>
            </ul>
          </template>
        </div>
      </nav>
    </div>
  </div>
</template>

<script>
import { defineComponent, computed } from 'vue';
import { useStore } from 'vuex';
import { auth } from '@/plugins/firebase.js';
import 'firebase/auth';

export default defineComponent({
  setup() {
    const store = useStore();
    const user = computed(() => store.getters.user);

    const signOut = () => {
      auth.signOut().then(() => {
        auth.onAuthStateChanged(() => {
          this.$router.push({ name: 'Login' });
        });
      });
    };

    return {
      user,
      signOut
    };
  }
});
</script>
