<template>
  <v-row>
    <v-col cols="12">
      <h2 class="text-center">{{ thisItemInfo[2] }}</h2>
    </v-col>
    <v-col cols="12" xs="12" sm="12" md="6" lg="6">
      <v-card class="max-auto" flat color="grey lighten-3">
        <v-container>
          <v-row>
            <v-col cols="12">
              <v-row justify="center" class="mx-1 mt-12 mb-6">
                <v-card class="max-auto" flat>
                  <v-img :src="require(`@/assets/sneakers/${this.$route.params.id}.jpg`)"></v-img>
                </v-card>
              </v-row>
            </v-col>
            <v-col cols="12">
              <v-row justify="center" class="mx-1">
                <v-btn v-if="!favoriteheart" class="mt-3 mb-9" outlined color="teal darken-3 white--text" @click="favoriteclick">
                  <span>Favorite</span>
                  <v-icon>mdi-heart</v-icon>
                </v-btn>
                <v-btn v-else class="mt-3 mb-9" color="teal darken-3 white--text" @click="favoriteclick">
                  <span>Favorite</span>
                  <v-icon>mdi-heart</v-icon>
                </v-btn>
              </v-row>
            </v-col>
          </v-row>
        </v-container>
      </v-card>
    </v-col>

    <v-col cols="12" xs="12" sm="12" md="6" lg="6" class="pt-0">
      <v-card class="max-auto" flat>
        <v-container class="pt-0">
          <v-row>
            <v-col cols="12">
              <h1 class="mb-3">{{ thisItemInfo[2] }}</h1>
              <h2 class="mb-1">가격: {{ thisItemInfo[1] }} XKRW</h2>
              <h2 class="mb-1">판매자: {{ pan }}</h2>
              <h2>토큰 아이디: {{ thisItemInfo[0] }}</h2>
            </v-col>
            <v-col cols="12">
              <h2 class="mb-3">제품 정보</h2>
              <h3>브랜드: {{ itemInfo.brand }}</h3>
              <h3>사이즈: {{ thisItemInfo[3] }}</h3>
              <h3>역대 판매 가격</h3>
              <h3>{{ thisItemInfo[5] }} XKRW</h3>
              <h3>역대 소유주 고유주소</h3>
              <h3>{{ thisItemInfo[6] }}</h3>
            </v-col>
            <v-col cols="12">
              <h2 class="mb-2">제품 설명</h2>
              <v-textarea name="input-7-1" label="제품 설명" auto-grow :value="sul" readonly></v-textarea>
            </v-col>
            <v-col cols="12">
              <v-row justify="end" class="mx-1">
                <v-btn color="teal darken-3 white--text" @click="$router.push({ name: 'ItemBuy', params: { id: thisItemInfo[0] } })">구매하기</v-btn>
              </v-row>
            </v-col>
          </v-row>
        </v-container>
      </v-card>
    </v-col>
  </v-row>
</template>

<script>
import { mapState } from 'vuex'
import klaytnService from '@/klaytn/klaytnService'
const service = new klaytnService()

export default {
  data() {
    return {
      favoriteheart: false,
      itemInfo: {},
      thisItemInfo: '',
      sul: '신발팝니다~',
      pan: '지드래곤',
    }
  },
  methods: {
    async getItemInfo() {
      this.thisItemInfo = await service.getProductInfo(this.$route.params.id)
    },
    favoriteclick() {
      this.favoriteheart = !this.favoriteheart
    },
  },
  computed: {
    ...mapState(['userInfo']),
  },
  async created() {
    this.getItemInfo()
  },
}
</script>

<style></style>
