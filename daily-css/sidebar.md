# Buat Sidebar CSS

cara manual untuk buat sidebar css sederhana untuk componen standar.

```html
<button class="side-pop-button">Click Me</button>
<div class="side-pop-content">
  <p>Content to be shown...</p>
</div>

```

###### buat style

```scss
.side-pop-content {
  position: fixed;
  top: 0;
  left: -100%;
  width: 300px;
  height: 100vh;
  background-color: #fff;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.2);
  padding: 20px;
  transition: left 0.3s ease;
}

.side-pop-content.show {
  left: 0;
}

```

###### buat script

```javascript
var button = document.querySelector('.side-pop-button');
var content = document.querySelector('.side-pop-content');

button.addEventListener('click', function() {
  content.classList.toggle('show');
});

```

kendala dari componens standar ini belum di buat button collabs jika ingin menutup sidbar,tapi konsepnya sama dengan toggle di class show
