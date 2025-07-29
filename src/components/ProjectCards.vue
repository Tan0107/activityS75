<template>
  <div class="col-xl-4 col-md-6 col-sm-12 mb-4 d-flex align-items-stretch">
    <div class="card shadow-sm border-0 project-card w-100">
      <!-- Image Container with Advanced Effects -->
      <div class="image-container">
        <img
          :src="project.image"
          class="card-img-top project-image"
          :alt="project.title"
          loading="lazy"
        />
        
        <!-- Gradient Overlay -->
        <div class="image-overlay"></div>
        
        <!-- Shimmer Effect -->
        <div class="shimmer-overlay"></div>
        
        <!-- Corner Accent -->
        <div class="corner-accent"></div>
      </div>

      <div class="card-body">
        <!-- Title with Gradient Underline -->
        <h5 class="card-title text-dark fw-semibold">
          {{ project.title }}
          <div class="title-underline"></div>
        </h5>
        
        <!-- Description with Enhanced Typography -->
        <p class="card-text text-secondary">{{ project.description }}</p>
        
        <!-- Decorative Elements -->
        <div class="card-decoration">
          <div class="decoration-dot"></div>
          <div class="decoration-line"></div>
        </div>
      </div>
      
      <!-- Floating Action Indicator -->
      <div class="floating-indicator">
        <div class="indicator-pulse"></div>
      </div>
    </div>
  </div>
</template>

<script setup>
defineProps({
  project: {
    type: Object,
    required: true
  }
});
</script>

