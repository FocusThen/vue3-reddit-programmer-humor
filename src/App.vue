<template>
<h1>Programmer Humor</h1>
<div v-for="post in posts" :key="post.id">
    <a :href="post.url">
        <h3>{{post.title}}</h3>
        <img v-if="post.image" :src="post.image" alt="">
    </a>
</div>
</template>

<script>
import {
    ref
} from 'vue'
export default {
    setup() {
        const posts = ref([])

        async function getPots() {
            const response = await fetch('https://www.reddit.com/r/ProgrammerHumor.json')
            const json = await response.json()
            console.log(json.data.children.map(child => child.data))
            posts.value = json.data.children.map(child => ({
                id: child.data.id,
                image: child.data.url,
                title: child.data.title,
                url: `https://www.reddit.com/${child.data.permalink}`
            }))
            console.log(posts.value)

        }
        getPots()

        return {
            posts
        }
    }
}
</script>

<style>
#app {
    font-family: Avenir, Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
    margin-top: 60px;
    max-width: 1368px;
    margin: 0 auto;
}

img {
    width: 30%;
}
</style>
