<template>
  <div>
    <div v-if="projectLoaded">
      <!-- Project card -->
      <article v-for="project in projects" :key="project._id" class="projectcard margin-b-30">
        <div>
          <router-link to="/project" @click="goToProject(project._id, project.title, project.collaborators)">
            <h3 class="margin-b-15">{{ project.title }}</h3>
          </router-link>
          <!-- Edit and delete buttons -->
          <div class="icon">
            <button @click="openModal(project._id)"><img src="../assets/pen-icon.svg" alt=""></button>
            <!-- Edit modal -->
            <ProjectEditModal :is-open="modalOpen" @close="modalOpen = false" />
            <button @click="deleteProject(project._id)"><img src="../assets/bin-icon.svg" alt=""></button>
          </div>
        </div>
        <router-link to="/project" @click="goToProject(project._id, project.title, project.collaborators)">
          <p class="margin-b-15">{{ project.description }}</p>
          <div class="date-flex">
            <p class="margin-b-15">{{ project.startDate }}</p>-
            <p class="margin-b-15">{{ project.endDate }}</p>
          </div>
          <div class="statusowner-flex">
            <p>{{ project.status }}</p>
            <p class="project-owner">{{ project.author }}</p>
          </div>
        </router-link>
      </article>
    </div>
    <div v-else>
      <p>Loading</p>
    </div>
  </div>
</template>

<script>
  import { mapMutations } from 'vuex';
  import ProjectEditModal from '../components/ProjectEditModal.vue';
  import getAllProjects from '../modules/project.js';
  import { ref, onMounted } from "vue";
  import store from '../store/store';
  import { useRouter } from 'vue-router';

  export default {
    name: 'ProjectCard',
    components: {
      ProjectEditModal
    },
    setup() {
      const { project, projects, projectLoaded, getProjectbyID, deleteProject } = getAllProjects();

      const router = useRouter();
      const modalOpen = ref(false);
      const openModal = (_id) => {
        if (_id) {
          modalOpen.value = true;
          store.commit('setProjectId', _id);
          console.log
        } else {
          modalOpen.value = true;
        }
      };
      const goToProject = (_id, title, collaborators) => {
        if (_id) {
         store.commit('setProjectId', _id);
          store.commit('setProjectTitle', title);
          store.commit('setCollaborators', collaborators);
          router.push('/project');
        }
      };

        onMounted(() => {
          getProjectbyID();
        });

      return { project, projects, modalOpen, openModal, projectLoaded, deleteProject, ...mapMutations(['setProjectId', 'setProjectTitle', "setCollaborators"]), goToProject };
    },
  };
</script>

<style scoped>
  .projectcard {
    padding: 30px 50px;
    background-color: var(--light-grey);
  }

  .projectcard>div {
    display: flex;
    justify-content: space-between;
  }

  .icon {
    display: grid;
   grid-template-columns: 1fr 1fr;
    column-gap: 15px;
  }

  button {
    background: none;
    border: none;
    height: fit-content;
    cursor: pointer;
  }

  a {
    color: #232323;
  }

  .date-flex {
    display: flex;
    gap: 15px;
  }

  .statusowner-flex {
    display: flex;
    justify-content: space-between;
  }

  .project-owner {
    color: var(--dark-green);
  }

  .taskmodal-background {
    position: fixed;
    left: 0;
    top: 0;
    width: 100%;
    height: 100%;
    background-color: rgb(0, 0, 0);
    background-color: rgba(0, 0, 0, 0.850);
    justify-content: center;
    align-items: center;
    padding: 50px 0;
  }

  .taskmodal {
    width: 750px;
    padding: 50px 50px;
    background-color: var(--light-grey);
  }

  .textcenter {
    text-align: center;
  }

  .collab-container {
    overflow-y: auto;
    height: 180px;
    background-color: rgb(133, 133, 133);
    padding: 15px 30px;
  }

  .collaborators {
    display: flex;
    justify-content: space-around;
  }
</style>
