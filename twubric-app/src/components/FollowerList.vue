<template>
  <div>
    <div class="mui-container">
      <div class="mui-row">
        <div class="mui-col-md-6">
          <div class="filters">
            <label>Sort By:</label>
            <button @click="sortBy('twubricScore')" class="mui-btn mui-btn--primary">Twubric Score</button>
            <button @click="sortBy('friends')" class="mui-btn mui-btn--primary">Friends</button>
            <button @click="sortBy('influence')" class="mui-btn mui-btn--primary">Influence</button>
            <button @click="sortBy('chirpiness')" class="mui-btn mui-btn--primary">Chirpiness</button>
          </div>
        </div>
        <div class="mui-col-md-6">
          <div class="filters">
            <label>Joined Twitter between:</label>
            <input type="date" v-model="startDate" @change="filterByDate" class="mui-textfield"/>
            <input type="date" v-model="endDate" @change="filterByDate" class="mui-textfield"/>
          </div>
        </div>
      </div>
    </div>
    <table class="mui-table mui-table--bordered">
      <thead>
        <tr>
          <th>Username</th>
          <th>Friends</th>
          <th>Twubric Score</th>
          <th>Influence</th>
          <th>Chirpiness</th>
          <th>Joined</th>
          <th>Actions</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="follower in filteredFollowers" :key="follower.uid">
          <td>{{ follower.username }}</td>
          <td>{{ follower.twubric.friends }}</td>
          <td>{{ follower.twubric.total }}</td>
          <td>{{ follower.twubric.influence }}</td>
          <td>{{ follower.twubric.chirpiness }}</td>
          <td>{{ formatDate(follower.join_date) }}</td>
          <td>
            <button @click="removeFollower(follower.uid)" class="mui-btn mui-btn--danger">Remove</button>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>


<script>
import { ref, onMounted, computed } from 'vue';
import twubricData from '../assets/twubric.json';

export default {
  setup() {
    const followers = ref([]);
    const startDate = ref(null);
    const endDate = ref(null);
    const sortByCriteria = ref(null);

    const loadFollowers = () => {
      followers.value = twubricData; 
    };

    const sortBy = (criteria) => {
      if (sortByCriteria.value === criteria) {
        followers.value.reverse();
      } else {
        followers.value.sort((a, b) => a.twubric[criteria] - b.twubric[criteria]);
        sortByCriteria.value = criteria;
      }
    };

    const filterByDate = () => {
      const filtered = twubricData.filter(follower => {
        const joinedDate = new Date(follower.join_date * 1000);
        return (!startDate.value || joinedDate >= new Date(startDate.value)) &&
               (!endDate.value || joinedDate <= new Date(endDate.value));
      });
      followers.value = filtered;
    };

    const removeFollower = (uid) => {
      followers.value = followers.value.filter(follower => follower.uid !== uid);
    };

    const formatDate = (timestamp) => {
      const date = new Date(timestamp * 1000);
      return date.toLocaleDateString();
    };

    const filteredFollowers = computed(() => {
      return followers.value.filter(follower => {
        const joinedDate = new Date(follower.join_date * 1000);
        return (!startDate.value || joinedDate >= new Date(startDate.value)) &&
               (!endDate.value || joinedDate <= new Date(endDate.value));
      });
    });

    onMounted(loadFollowers);

    return { followers, startDate, endDate, sortBy, filterByDate, removeFollower, filteredFollowers, formatDate };
  }
};
</script>

<style>
.filters{
  margin: 12px;
  display: flex;
  gap: 30px;
}
.mui-textfield {
  margin-right: 10px;
}

/* MUI table styles */
.mui-table {
  width: 100%;
  border-collapse: collapse;
}

.mui-table th,
.mui-table td {
  padding: 8px;
  text-align: left;
  border-bottom: 1px solid #ddd;
}

.mui-table th {
  background-color: #f2f2f2;
}

/* MUI table button styles */
.mui-btn--danger {
  background-color: #f44336;
  color: white;
  border: none;
  padding: 6px 12px;
  cursor: pointer;
  border-radius: 4px;
  text-transform: uppercase;
}

.mui-btn--danger:hover {
  background-color: #d32f2f;
}

.mui-btn--primary {
  background-color: #2196f3;
  color: white;
  border: none;
  padding: 6px 12px;
  cursor: pointer;
  border-radius: 4px;
  text-transform: uppercase;
}

.mui-btn--primary:hover {
  background-color: #1976d2;
}
</style>
