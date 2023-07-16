---
layout: default
---

<div class="slideshow-container">
{% for image in site.static_files %}
  {% if image.path contains 'slideshow/' %}
    <div class="mySlides">
      <img src="{{ site.baseurl }}{{ image.path }}" alt="{{ image.name }}">
    </div>
  {% endif %}
{% endfor %}
</div>

<script>
var slideIndex = 0;
showSlides();

function showSlides() {
  var i;
  var slides = document.getElementsByClassName("mySlides");
  for (i = 0; i < slides.length; i++) {
    slides[i].style.display = "none";
  }
  slideIndex++;
  if (slideIndex > slides.length) {
    slideIndex = 1;
  }
  slides[slideIndex - 1].style.display = "block";
  setTimeout(showSlides, 2000); // Altere o tempo de exibição dos slides (em milissegundos) aqui
}
</script>

<style>
.slideshow-container {
  max-width: 1000px;
  position: relative;
  margin: auto;
}

.mySlides {
  display: none;
}

img {
  width: 100%;
  height: auto;
}
</style>
