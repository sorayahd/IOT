<template>
  <div class="dashboard">
    <h1>Dashboard</h1>
    <div>
      <template v-for="(emotion, index) in emotions">
        <div :key="index">
<center>
          <table class="table-striped">
      <tr>
        <th>Emotions</th> 
        <th>Nombre   </th>
      </tr>
      <tr>
        <td>{{index}}</td>
        <td>{{emotion}}</td>
      </tr>
      
    </table>
  </center>

          <!-- <strong>{{index}}</strong> Nombre de personnes: {{ emotion }} -->
        </div>
      </template>
    </div>
  </div>
</template>

<script>
export default {
  name: "Dashboard",
  data: function(){
    return {
      emotions: {
        angry: 0,
        neutral: 0,
        happy: 0
      },
      pusher_obj: null,
      e_channel: null,
    }
  },
  mounted: function(){
    this.init();
  },
  methods: {
    init (){
      
      this.pusher_obj = new Pusher('3d165c0f2a879405873c',{
          cluster: 'eu',
          encrypted: true
      });
  
      this.e_channel = this.pusher_obj.subscribe('emotion_channel');
     
      let self = this;
      this.e_channel.bind('new_emotion', function(data) {
      
        self.emotions[`${data.emotion}`] += 1;
      });
    },
  },
}
</script>