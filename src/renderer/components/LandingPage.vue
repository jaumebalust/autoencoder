<template>
  <div id="wrapper">


      <div class="left-side">
        <span class="title">
          Autoencoder in progress...
        </span>
      </div>

      <div>

        <md-button class="md-raised md-primary" @click="preventSleeping">Prevent Display from Sleeping</md-button>

        <br><br>
        <md-button class="md-raised md-accent" @click="disableBlocker">Disable Blocker</md-button>
      </div>
    <system-information></system-information>

  </div>
</template>

<script>
  import SystemInformation from './LandingPage/SystemInformation'
  import electron from 'electron'
  export default {
    name: 'landing-page',
    components: { SystemInformation },
    data(){
      return {
        id:undefined,
        powerSaveBlocker:electron.remote.powerSaveBlocker
      }
    },
    methods: {
      open (link) {
        this.$electron.shell.openExternal(link)
      },
      disableBlocker(){
        if (this.id && this.powerSaveBlocker.isStarted(this.id)) {
          console.log('Prevent Display Sleep is Active');
          this.powerSaveBlocker.stop(this.id);
          console.log('Prevent Display Sleep is successfully disabled');
        } else {
          console.log('Prevent Display Sleep is not Active')
        }
      },
      preventSleeping(){
        // Importing powerSaveBlocker Module from remote module

        this.id = this.powerSaveBlocker.start('prevent-display-sleep');
        console.log('Request ID generated - ', this.id);
        console.log('Prevent Display Sleep is Enabled')
      }
    },
    computed:{
      isPrevented(){
        return this.id && this.powerSaveBlocker.isStarted(this.id)
      }
    }

  }
</script>

<style>
  @import url('https://fonts.googleapis.com/css?family=Source+Sans+Pro');

  * {
    box-sizing: border-box;
    margin: 0;
    padding: 0;
  }

  body { font-family: 'Source Sans Pro', sans-serif; }

  #wrapper {
    background:
      radial-gradient(
        ellipse at top left,
        rgba(255, 255, 255, 1) 40%,
        rgba(229, 229, 229, .9) 100%
      );
    height: 100vh;
    padding: 60px 80px;
    width: 100vw;
  }

  #logo {
    height: auto;
    margin-bottom: 20px;
    width: 420px;
  }

  main {
    display: flex;
    justify-content: space-between;
  }

  main > div { flex-basis: 50%; }

  .left-side {
    display: flex;
    flex-direction: column;
  }

  .welcome {
    color: #555;
    font-size: 23px;
    margin-bottom: 10px;
  }

  .title {
    color: #2c3e50;
    font-size: 20px;
    font-weight: bold;
    margin-bottom: 6px;
  }

  .title.alt {
    font-size: 18px;
    margin-bottom: 10px;
  }

  .doc p {
    color: black;
    margin-bottom: 10px;
  }

  .doc button {
    font-size: .8em;
    cursor: pointer;
    outline: none;
    padding: 0.75em 2em;
    border-radius: 2em;
    display: inline-block;
    color: #fff;
    background-color: #4fc08d;
    transition: all 0.15s ease;
    box-sizing: border-box;
    border: 1px solid #4fc08d;
  }

  .doc button.alt {
    color: #42b983;
    background-color: transparent;
  }
</style>
