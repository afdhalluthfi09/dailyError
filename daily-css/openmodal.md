# Open Modal Mode Backdrop Statik Manual

berikut styling dari open modal manual yang saya gunakan

##### html

```
<button id="openModalBtn">click modal</button>
<div id="myModal" class="modal">
        <div class="modal-content">
            <span class="close">×</span>
            <h2>Modal Title</h2>
            <p>Isi dari modal...</p>
        </div>
</div>
```

##### scss

```
.modal {
    display: none;
    position: fixed;
    z-index: 999;
    left: 0;
    top: 0;
    width: 100%;
    height: 100%;
    overflow: auto;
    background-color: rgba(0, 0, 0, 0.4);

    @keyframes modalopen {
        from {
            opacity: 0;
            transform: scale(0.8);
        }
        to {
            opacity: 1;
            transform: scale(1);
        }
    }
    .modal-content {
        background-color: #fefefe;
        margin: 10% auto;
        padding: 20px;
        border: 1px solid #888;
        width: 80%;
        animation-name: modalopen;
        animation-duration: 0.5s;
        .close {
            color: #aaa;
            float: right;
            font-size: 28px;
            font-weight: bold;
            cursor: pointer;
            &:hover {
                color: $color_2;
                text-decoration: none;
                cursor: pointer;
            }
            &:focus {
                color: $color_2;
                text-decoration: none;
                cursor: pointer;
            }
          }
      }
  }
```

##### script js

```
let openModalBtn = document.getElementById("openModalBtn");
        let modal = document.getElementById("myModal");
        let closeBtn = document.getElementsByClassName("close")[0];
        openModalBtn.addEventListener("click", function() {
            modal.style.display = "block";
        });

        closeBtn.addEventListener("click", function() {
            modal.style.display = "none";
        });
```
