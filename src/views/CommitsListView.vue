<template>
  <div class="repo-view">
    <h1>All commits</h1>
    <spinner :show="isLoading"></spinner>
    <ul>
      <li v-if="commit.committer" class="commit-item" v-for="commit in commits">
        <img :src="commit.committer.avatar_url">
        <div class="commit-item-desc">
          <p class="commit-message">{{ commit.commit.message | truncate(100) }}</p>
          <p><b>{{ commit.commit.author.name }}</b> committed on {{ commit.commit.author.date | timeAgo }}</p>
        </div>
      </li>
    </ul>
  </div>
</template>

<script>
  import {truncate, timeAgo} from '../filters'
  import Spinner from '../components/Spinner.vue'

  export default {
    name: 'repo-detail',
    beforeMount: function () {
      let owner = this.$route.params.owner;
      let repo = this.$route.params.repo;
      let url = `https://api.github.com/repos/${owner}/${repo}/commits`;
      this.$http.get(url).then((response) => {
        this.commits = response.body;
        this.isLoading = false;
      });
    },
    data: function () {
      return {
        commits: [],
        isLoading: true
      }
    },

    components: {Spinner},
    filters: {truncate, timeAgo}
  }
</script>

<style>
  li.commit-item {
    display: flex;
    margin-bottom: 1.5em;
  }

  .commit-item img {
    width: 50px;
    height: 50px;
    margin-right: 2em;
  }

  .commit-item-desc {
    width: 100%;
  }

  .commit-message {
    margin-top: 0;
  }
</style>