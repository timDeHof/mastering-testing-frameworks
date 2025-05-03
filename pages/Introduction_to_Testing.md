---
title: Introduction to Testing
name: Intro
layout: image-right
image: ./assets/testing-pyramid.svg
transition: slide-up
---

# Introduction to Testing

<v-click>

## What is testing?

<v-click>

- **Unit Testing**  
  Isolated component/function validation  
  *Example: Testing a single utility function*

- **Integration Testing**  
  Component interaction verification  
  *Example: Testing API service + database layer*

- **End-to-End (E2E)**  
  Full user journey validation  
  *Example: Testing checkout flow from cart to payment*

</v-click>

</v-click>

---
layout: image-right
image: ./assets/testing-pyramid.svg
---

<Transition :name="transition">
<div class="grid grid-cols-1 gap-4">
<div>

# Why test?

<v-click>

<ul>
<li>🛠️ Ensure code reliability</li>
<li> 🐛 Catch bugs pre-production</li>
<li> 📚 Living documentation</li>
<li>🔄 Safe refactoring</li>
<li> 🤝 Team collaboration catalyst</li>
<li>⚡ Accelerate development cycle </li>
</ul>
</v-click>

</div>
<div class="flex items-center justify-center">
  <img :src="image" class="h-60" />
</div>
</div>
</Transition>
