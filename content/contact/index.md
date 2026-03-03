---
title: "Contact"
description: "Send a message without publishing a direct email address."
showAuthor: false
showDate: false
showWordCount: false
showReadingTime: false
---

Use the form below to send a message without exposing a direct email address.

<div class="contact-form-wrap">
  <form id="contact-form" class="contact-form" method="POST" action="https://formspree.io/f/mykdrzdg">
    <input type="hidden" name="_subject" value="New portfolio contact request">
    <input type="text" name="_gotcha" tabindex="-1" autocomplete="off" style="display:none">
    <label for="name">Name
      <input id="name" name="name" type="text" autocomplete="name" required>
    </label>
    <label for="email">Email
      <input id="email" name="email" type="email" autocomplete="email" required>
    </label>
    <label for="subject">Subject
      <input id="subject" name="subject" type="text" required>
    </label>
    <label for="message">Message
      <textarea id="message" name="message" rows="7" required></textarea>
    </label>
    <button type="submit">Send Message</button>
  </form>
  <p id="contact-status" class="contact-status" role="status" aria-live="polite"></p>
</div>

<script>
  (() => {
    const form = document.getElementById("contact-form");
    const status = document.getElementById("contact-status");

    if (!form || !status) return;

    const showStatus = (message, kind) => {
      status.textContent = message;
      status.classList.remove("contact-status--ok", "contact-status--error");
      if (kind === "ok") status.classList.add("contact-status--ok");
      if (kind === "error") status.classList.add("contact-status--error");
    };

    form.addEventListener("submit", async (event) => {
      event.preventDefault();
      showStatus("Sending...", "");

      const formData = new FormData(form);
      try {
        const response = await fetch(form.action, {
          method: "POST",
          body: formData,
          headers: { Accept: "application/json" },
        });

        if (response.ok) {
          form.reset();
          showStatus("Message sent successfully. I will get back to you soon.", "ok");
          return;
        }

        const payload = await response.json().catch(() => null);
        const errorMessage =
          payload?.errors?.map((error) => error.message).join(" ") ||
          "Submission failed. Please try again in a minute.";
        showStatus(errorMessage, "error");
      } catch (error) {
        showStatus("Network error. Please check your connection and try again.", "error");
      }
    });
  })();
</script>
