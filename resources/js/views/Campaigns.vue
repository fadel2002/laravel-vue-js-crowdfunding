<template>
    <div>
        <v-container class="pa-0" grid-list-sm>
            <v-subheader> All Campaigns </v-subheader>
            <v-layout wrap>
                <v-flex
                    v-for="campaign in campaigns"
                    :key="`campaign-` + campaign.id"
                    xs6
                >
                    <campaign-item :campaign="campaign" />
                </v-flex>
            </v-layout>
            <v-pagination
                v-model="page"
                @input="go"
                :length="lengthPage"
                :total-visible="6"
            ></v-pagination>
        </v-container>
    </div>
</template>
<script>
export default {
    data: () => ({
        campaigns: [],
        page: 0,
        lengthPage: 0,
    }),
    components: {
        CampaignItem: () => import("../components/CampaignItem"),
    },
    created() {
        this.go();
    },
    methods: {
        go() {
            let url = "api/campaign?page=" + this.page;
            axios
                .get(url)
                .then((response) => {
                    let { data } = response.data;
                    this.campaigns = data.campaigns.data;
                    this.page = data.campaigns.current_page;
                    this.lengthPage = data.campaigns.last_page;
                })
                .catch((err) => {
                    let { response } = err;
                    console.log(response);
                });
        },
    },
};
</script>
