<template>
  <table class="table">
    <tr class="table__header">
      <th class="table__th">対戦キラー</th>
      <th class="table__th">獲得得点</th>
      <th class="table__th">使用パーク</th>
      <th class="table__th">脱出人数</th>
    </tr>

    <tr
      v-for="result in results"
      :key="result.id"
      class="table__tr"
      :class="{ 'table__tr--win': result.win, 'table__tr--lose': result.lose }"
    >
      <td class="table__td table__name">
        <img class="table__img" :src="result.killerImage" alt="" />
        {{ result.killerName }}
      </td>
      <td class="table__td">{{ result.score }}</td>
      <td class="table__td">
        <img class="table__image" :src="result.parkImage01" alt="" />
        <img class="table__image" :src="result.parkImage02" alt="" />
        <img class="table__image" :src="result.parkImage03" alt="" />
        <img class="table__image" :src="result.parkImage04" alt="" />
      </td>
      <td class="table__td">{{ result.escape + '人' }}</td>
    </tr>
  </table>
</template>

<script>
import { API } from 'aws-amplify'
import { searchResultss } from '../../graphql/queries'
export default {
  data() {
    return {
      results: {}
    }
  },
  created() {
    this.getReadmes()
  },
  methods: {
    async getReadmes() {
      const listResults = await API.graphql({
        query: searchResultss,
        variables: {
          sort: {
            field: 'createdAt',
            direction: 'desc'
          },
          limit: 30
        }
      })
      this.results = listResults.data.searchResultss.items
    }
  }
}
</script>

<style lang="scss" scoped>
.table {
  width: 80%;
  margin: 5px auto 50px;
  font-size: 1.6rem;
  color: #fff;

  &__header {
    position: fixed;
    top: 120px;
    width: 80%;
  }

  &__th {
    width: 300px;
    padding: 0 0 20px;
    text-align: center;
    border-bottom: 1px solid #fff;
    // position: fixed;
    // top: 0;
    z-index: 10;
    background-image: url('~@/assets/img/bg.jpg');
    background-attachment: fixed;
    background-size: cover;
    background-position: center;
  }

  &__tr--win {
    background-color: rgba(255, 0, 0, 0.2);
  }
  &__tr--lose {
    background-color: rgba(0, 0, 255, 0.2);
  }

  &__img {
    width: 50px;
    height: 50px;
    display: flex;
    margin: 0 auto 5px;
  }

  &__image {
    width: 50px;
    height: 50px;
    margin: 0 auto;
  }

  &__name {
    font-size: 1.2rem;
  }

  &__td {
    width: 20%;
    padding: 14px 0 7px;
    text-align: center;
    border-bottom: 1px solid #fff;
  }
}
</style>
