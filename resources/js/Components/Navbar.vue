<script>
import { Link } from '@inertiajs/vue3';
import { User } from 'lucide-vue-next'; // Lucide User ikon importálása

export default {
  components: { Link, User },
  props: {
    user: Object,
  },
  computed: {
    userRole() {
      return this.user?.role || "guest";
    },
    userProfileImage() {
      return this.user?.profilePicture || null;
    }
  },
  methods: {
    logout() {
      this.$inertia.post(route('logout'));
    }
  }
};
</script>

<template>
  <nav class="bg-gray-900 text-white py-4 shadow-lg border-b-4 border-blue-500">
    <div class="container mx-auto flex justify-between items-center px-6">
      
      <!-- Logo -->
      <Link :href="route('dashboard')" class="flex items-center space-x-3">
        <img src="@/Layouts/logo.jpg" alt="Triviaverse Logo" class="h-10 rounded-full shadow-md">
        <span class="text-2xl font-bold text-blue-400 hover:text-blue-500 transition">
          Triviaverse
        </span>
      </Link>

      <!-- Menü -->
      <ul class="hidden md:flex space-x-6 text-lg">
        <li v-if="userRole !== 'student'">
          <Link 
            :href="route('quizzes.create')" 
            class="hover:text-blue-400 transition duration-300"
          >
            Kvíz létrehozása
          </Link>
        </li>
        <li>
          <Link 
            :href="route('quizzes.index')" 
            class="hover:text-blue-400 transition duration-300"
          >
            Kvízek
          </Link>
        </li>
        <li v-if="userRole === 'admin'">
          <Link 
            :href="route('quizzes.manage')" 
            class="hover:text-blue-400 transition duration-300"
          >
            Kvízek kezelése
          </Link>
        </li>
      </ul>

      <!-- Felhasználói menü -->
      <div class="flex items-center space-x-6">
        <Link :href="route('profile.edit')" class="flex items-center">
          <img 
            v-if="userProfileImage"
            :src="userProfileImage"
            alt="Profilkép"
            class="w-10 h-10 rounded-full border-2 border-blue-500 shadow-lg cursor-pointer"
          />
          <User v-else class="w-10 h-10 text-gray-400 border-2 border-blue-500 rounded-full p-1 shadow-lg cursor-pointer" />
        </Link>
        <button 
          @click="logout" 
          class="bg-red-500 hover:bg-red-600 text-white px-5 py-2 rounded-lg shadow-md transition transform hover:scale-105"
        >
          Kijelentkezés
        </button>
      </div>
    </div>
  </nav>
</template>
