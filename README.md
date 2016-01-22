# vue-datetimepicker [![npm package](https://img.shields.io/npm/v/vue-datetimepicker.svg)](https://www.npmjs.com/package/vue-datetimepicker)

> datetimepicker component for Vue.js

# Requirements

- [Vue.js](https://github.com/yyx990803/vue) `^1.0.0`

# Instllation

## npm 
``` bash
$ npm install vue-datetimepicker
```

# Usage
``` html
<template>
    <datetime :readonly="true" format="YYYY-MM-DD"></datetime>
    <datetime format="YYYY-M-D" value="2015-9-5"></datetime>
    <datetime :readonly="true" format="MMM/D/YYYY" width="300px"></datetime>
</template>

<script>
import datetime from 'vue-datetimepicker';

export default {
    components: { datetime }
};
</script>
```

# License

[The MIT License](http://opensource.org/licenses/MIT)