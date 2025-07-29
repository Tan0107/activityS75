<template>
	<section id="contact" >
    <div class="container bg-white">
      <h2>Contact Me</h2>
      <p>Let's get in touch!</p>

      <form class="mt-4 mx-auto contact-form mb-3">
        <div class="mb-3 text-start">
          <label for="name" class="form-label">Name</label>
          <input type="text" class="form-control" id="name" required>
        </div>

        <div class="mb-3 text-start">
          <label for="email" class="form-label">Email</label>
          <input type="email" class="form-control" id="email" required>
        </div>

        <div class="mb-3 text-start">
          <label for="message" class="form-label">Message</label>
          <textarea class="form-control" id="message" rows="4" required></textarea>
        </div>

        <button type="submit" class="btn btn-primary">Send</button>
      </form>
    </div>
  </section>
</template>