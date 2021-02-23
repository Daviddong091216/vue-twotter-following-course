<template>
  <div class="user-profile">
    <div class="user-profile_user-panel">
      <h1 class="user-peofile_username">@{{ user.username }}</h1>
      <div class="user-profile_admin-badge" v-if="user.isAdmin">
          Admin
      </div>
      <div class="user-profile_follower-count">
        <strong>Followers: </strong> {{ followers }}
      </div>
    </div>
    <div class="user-profile_twoots-wrapper">
        <div class="user-profile_twoot" v-bind:key="twoot.id" v-for="twoot in user.twoots">
            {{twoot.content}}
        </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      followers: 0,
      user: {
        id: 1,
        username: "DavidDong",
        firstName: "David",
        lastName: "Dong",
        email: "daviddong201612@gmail.com",
        isAdmin: true,
        twoots: [
            { id: 1, content: 'Twotter is Amazing!' },
            { id: 2, content: 'I love you Miley!'}
        ],
      },
    };
  },
  watch: {
    followers(newFollowerCount, oldFollowerCount) {
      if(oldFollowerCount < newFollowerCount) {
        console.log(`${this.user.username} has gained a follower!`);
      }
    }
  },
  computed: {
    fullName() {
      return `${this.user.firstName} ${this.user.lastName}`;
    },
  },
  methods: {
    followUser() {
      this.followers++;
    },
  },
  mounted() {
    this.followUser();
  }
};
</script>

<style>
.user-profile {
    display: grid;
    grid-template-columns: 1fr 3fr;
    width: 100%;
    padding: 50px 5%;
}
.user-profile_user-panel {
    display: flex;
    flex-direction: column;
    margin-right: 50px;
    padding: 20px;
    background-color: white;
    border-radius: 5px;
    border: 1px sold #DFE3E8;
}
.user-profile_admin-badge {
    background: darkgreen;
    color:white;
    border-radius: 5px;
    margin-right: auto;
    padding: 0 10px;
    font-weight: bold;
}

h1 {
    margin: 0;
}
</style>