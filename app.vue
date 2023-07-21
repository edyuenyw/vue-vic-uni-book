<template>
  <div>
    <!-- <h2>{{ post.id }} {{ post.title }}</h2> -->
    <!-- <p>{{ post.body }}</p> -->
    <ul>
      <li v-for="post in posts" :key="post.id">
        <!-- data[0].relationships.field_book_cover.links.self.href -->
        <!-- <img :src="post.relationships.field_book_cover.data.id"> -->
        <div>
          {{ post.id }}
        </div>
        <div>
          {{ post.title }}
        </div>
        <div>
          {{ post.field_author }}
        </div>
        <div>
          {{ post.filename }}
        </div>
        <div>
          <img style="height: 300px;" :src="post.fileurl">
        </div>
      </li>
    </ul>
  </div>
</template>

<script>
import axios from 'axios';
export default {
  mounted() {
    axios
      .get('https://vic-uni-book.lndo.site/jsonapi/node/books?include=field_book_cover')
      .then((response) => {
        // console.log(response.data.data)
        // this.posts = response.data.data
        const results = response.data.data
        console.log(results)
        const included = response.data.included
        console.log(included)
        const foundId = included.find(element => element.id = 'd89eb033-0307-44d8-a4ec-154c274a9175')
        // attributes.filename
        console.log(foundId.attributes.filename)
        this.posts = results.map(post => ({
          id: post.relationships.field_book_cover.data.id,
          title: post.attributes.title,
          field_author: post.attributes.field_author,
          // thumbnail: 'http://vic-uni-book.lndo.site/sites/default/files/2023-07/HTML-and-CSS-Design-and-Build-Websites-1st-Edition.jpg'
          // [0].relationships.field_book_cover.data.id
          filename: included.find(element => element.id === post.relationships.field_book_cover.data.id).attributes.filename,
          fileurl: "https://vic-uni-book.lndo.site" + included.find(element => element.id === post.relationships.field_book_cover.data.id).attributes.uri.url,
        }))
      })
  }
}
</script>