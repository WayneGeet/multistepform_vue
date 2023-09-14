<template>
    <h2>{{title}}</h2>
    <p>{{sub}}</p>
    <slot name="form"></slot>
    <slot name="plan"></slot>
    <slot name="billing"></slot>
</template>

<script>
import { ref } from 'vue'
export default {
    props:["title", "sub"],
    setup(props){
        
        return{}

    }
}
</script>

<style>

</style>