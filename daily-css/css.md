# ini daily-css pribadi untuk mempermudah kerjaan saya 
### Reset Default css browser
```scss
  * {
    margin: 0;
    padding: 0;
    border: 0;
    font-size: 100%;
    font: inherit;
    box-sizing: border-box;
    &::before {
      box-sizing: border-box;
    }
    &::after {
      box-sizing: border-box;
    }
}
```
## 1. css container yang sering saya pakai
```scss
    .container{
        width: min(100% - 1em, 69em);
        margin-inline: auto;
        border: 1px solid #000;
    }
```
## 2. breakpoint untuk response dan cara penggunaanya
```scss
    $breakpoints : (
        'small' : 576px,
        'medium' : 768px,
        'large' : 1024px,
        'xlarge' : 1200px,
        'xxlarge' : 1440px
    );

@mixin medium {
  @media screen and (min-width: map-get($breakpoints, "medium")) {
    @content;
  }
}

@mixin small {
  @media screen and (max-width: map-get($breakpoints, "small")) {
    @content;
  }
}

@mixin breakpoint($bp:0){
    @media (max-width: map-get($breakpoints, $bp)) {
        @content;
    }
}
/** atau dengan cara seperti dibawah ini */
$small:540px;
@media screen and (max-width: $small) {
    .yourClassName{}
}
```
## 2.1 Cara Panggil Dari Breakpoint
```scss
  @include small{
    .yourclass{}
  }
  @include medium{
    .yourclass{}
  }
  @include breakpoint{
    .yourclass{}
  }
```

