<template>
  <div class="mainContainer">
    <NavBar/>
    <div class="mainBg">
      <div class="titleContainer">
        <Title title-name="Mis Favoritos" to="/main"/>
      </div>
      <div v-if="ready" class="routineContainer">
        <Routine
            purple
            :is-mine="routine.user.id.toString() === store.getUserId()"
            class="routine"
            v-for="(routine) in routines"
            :key="routine.id"
            :title="routine.name"
            :rating="routine.averageRating"
            :owner="routine.user.username"
            :owner-img="routine.user.avatarUrl"
            :description="routine.detail"
            :difficulty="routine.difficulty"
            :id="routine.id"
            :is-public="routine.isPublic"
            @deleteRoutine="deleteRoutine($event)"
            @unfav="unfav($event)"
        />
      </div>
    </div>
    <Footer/>
  </div>
</template>

<script>
import NavBar from "@/components/NavBar";
import Footer from "@/components/Footer";
import Title from "@/components/Title";
import Routine from "@/components/Routine";
import {RoutineApi} from "@/backend/routines";
import UserStore from "@/stores/UserStore";
import router from "@/routes";
import {FavouritesApi} from "@/backend/favourites";
export default {
  name: "MyFavourites",
  components: {Routine, Title, Footer, NavBar},
  data() {
    return {
      store: UserStore,
      routines: undefined,
      ready:false
    }
  },
  async created() {
    if (!this.store.isLoggedIn()) {
      router.push("/permissionDenied");
    }
    const data = await FavouritesApi.getFavourites();
    this.routines = data.content;
    for (let routine of this.routines){
      const newRoutine = await RoutineApi.getRoutineById(routine.id);
      routine.user = newRoutine.user;
    }
    this.ready = true

  },
  methods:{
    deleteRoutine: async function(id){
      try{
        await RoutineApi.deleteRoutine(id)
        this.routines.splice(this.routines.findIndex(routine => routine.id === id), 1);
      }catch (e){
        alert(e);
      }
    },
    unfav(id){
      this.routines.splice(this.routines.findIndex(routine => routine.id === id), 1);
    }
  }


}
</script>



<style scoped>
div{
  overflow-x: hidden
}

.mainContainer{
  display: flex;
  min-height: 100vh;
  flex-direction: column;
  max-width: 100vw;
}

.mainBg{
  flex: 1;
  background-color: #f3f9f9;
  padding: 30px 35px 90px 35px;
  display: flex;
  flex-direction: column;
  color: #399c70;
}

.routineContainer{
  display: flex;
  flex-wrap: wrap;
  justify-content: space-evenly;
  margin-top: 20px;
  padding: 0 15px 0 15px;
}

.routine{
  margin-bottom: 30px;
}

.titleContainer{
  display: flex;
  width: 100%;
}

button{
  color: #35a371;
  margin-right: 10px;
  text-align: center;
  padding: 8px 35px 8px 35px;
  background-color: inherit;
  border: 4px solid #399c70;
  border-radius: 25px;
  font-size: 26px;
  font-weight: 700;
  text-decoration: none;
  outline: none;
  transition: 0.3s ease-in-out;
  margin-bottom: 10px;
  cursor: pointer;
}
button:hover{
  transition: 0.3s ease-in-out;
  background-color: #dbefe7;
  color: #156844;
}

a{
  width: 320px;
}
</style>