<template>
    <div id="printouts">
        <div class="page-title-container">
            <div class="container">
                <h1 id="page-title">
                    <span class="lang-fr">Affiches à imprimer</span>
                    <span class="lang-he">פוסטרים להדפסה</span>
                </h1>
            </div>
        </div>
        <div class="container">
            <h1 id="page-title"></h1>
            <div id="page-content"></div>
        </div>
    </div>
</template>

<style lang="scss">
    #printouts {
        
    }
</style>

<script>
export default {
    name: 'Printouts',
    metaInfo() {
        return {
            title: this.pageTitle            
        }
    },
    data() {
        return { 
            elements: [],
            pageTitle: 'Fiches à Imprimer',
            pageTitleHe: 'גליונות להדפסה'
        }
    },
    created() {
        this.getContent()
        this.setPageTitle()
    },
    mounted() {
         this.$root.$on('langChanged', this.getContent);
    },
    methods: {
        getContent() {
            this.$flamelinkApp.content.get({
                schemaKey: 'pages',
                entryId: this.$route.meta.entryId
            })
            .then(pageContent => {
                // document.getElementById('page-title').innerText = pageContent.title;
                document.getElementById('page-content').innerHTML = pageContent.content;
            })
            .catch(error => console.error('Something went wrong while retrieving the entry. Details:', error));
        },
        setPageTitle() {
            this.$flamelinkApp.settings.getLocale()
            .then(locale => {
                if (locale === 'he') {
                    this.pageTitle = this.pageTitleHe
                }
            })
        }
    }
};
</script>