<template lang="pug">
    LayoutDefault
        section.page-section
            h1.page-section__title {{ title }}

            CategoriesList(
                title="Navegue por categoria:"
                :categories="categories"
            )

            hr

            ListPosts(
                v-if="posts"
                title="Últimos Posts" 
                description=""
                :posts="posts"
                :query="get_posts"
                :pagination="pagination"
            )

            Preload(v-else)

            


</template>

<script>
import LayoutDefault from '@/components/Layouts/LayoutDefault'
import ListPosts from '@/components/ListPosts/ListPost'
import Preload from '@/components/Preload/Index'

export default {
    name: 'IndexPage',
    components: {
        LayoutDefault,
        ListPosts,
        Preload
    },
    data(){
        return {
            title: 'Bem vindo ao blog da Apiki',
            get_posts: '?_embed&categories=518',
        }
    },
    methods: {
        async tras_posts(params, page) {
            if( !params ) return;
            const bool_load_page = true
            await this.$store.dispatch( 'posts/request_posts', { params, page, bool_load_page } );

        },
        async tras_all_categories() {
            await this.$store.dispatch( 'categories/request_categs', {} );
        }
    },
    created() {
        this.tras_posts(this.get_posts, null);
        this.tras_all_categories();
    },
    computed: {
        posts() {
            const posts_arr = this.$store.state.posts.posts
            return posts_arr.length > 0 ? posts_arr : null
        },
        categories() {
            return this.$store.state.categories.categories
        },
        pagination() {
            return this.$store.state.posts.pagination
        }
    }
}
</script>
