# mpvue-ripple

> mpvue-ripple is a ripple component for mpvue.

## Screenshots

![](./screenshots.gif)

## Install

```bash
npm install mpvue-ripple --save
```

## Usage

```html
<template>
  <div>
    <ripple ripple-class="ripple-test">
      <div>ripple test</div>
    </ripple>
  </div>
</template>

<script>
import ripple from 'mpvue-ripple'

export default {
  components: {
    ripple
  }
}
</script>

<style>
page {
  background: #f0f0f0;
}
.ripple-test {
  margin-top: 10px;
  background: #fff;
  text-align: center;
  padding: 12px;
  border-top: 0.5px solid #ddd;
  border-bottom: 0.5px solid #ddd;
}
</style>
```
