<template>
  <div id="app">
    <button type="button" @click="socialName = ''">Reset social filter</button>
    <div class="social-links-navigation">
      <button type="button" @click="socialName = 'Instagram'">Instagram</button>
      <button type="button" @click="socialName = 'YouTube'">YouTube</button>
      <button type="button" @click="socialName = 'Facebook'">Facebook</button>
      <button type="button" @click="socialName = 'Twitter'">Twitter</button>
    </div>


    <tabs
      :tabs="tabs"
      :currentTab="currentTab"
      :wrapper-class="'default-tabs'"
      :tab-class="'default-tabs__item'"
      :tab-active-class="'default-tabs__item_active'"
      :line-class="'default-tabs__active-line'"
      @onClick="handleClick"
    />

    <div class="content" v-if="cards.length > 0">
      <div class="content-tab" v-if="currentTab === 'tab1'">
        <div class="card" v-for="(card, i) in socialComputed" :key="i">
          <Card :card="card" :status="''" @update-status="updateStatus"></Card>
        </div>
      </div>
      <div class="content-tab" v-if="currentTab === 'tab2'">
       <div class="card" v-for="(card, i) in updatedApproved" :key="i">
          <Card :card="card" :status="'approve'" @update-status="updateStatus"></Card>
        </div>
      </div>
      <div class="content-tab" v-if="currentTab === 'tab3'">
        <div class="card" v-for="(card, i) in updatedRejected" :key="i">
          <Card :card="card" :status="'reject'" @update-status="updateStatus"></Card>
        </div>
      </div>
    </div>
    <div class="cards-empty" v-else>Posts is loading...</div>
  </div>
</template>

<script>
  import Tabs from './components/vueTabs';
  import Card from './components/Card';

  const TABS = [{
    title: 'Pending',
    value: 'tab1',
  }, {
    title: 'Approved',
    value: 'tab2',
  }, {
    title: 'Rejected',
    value: 'tab3',
  }];

  export default {
    name: 'app',
    props: ['card'],
    components: {
      Tabs,
      Card
    },
    data(){
      return {
        tabs: TABS,
        currentTab: 'tab1',
        cards: [],
        socialName: '',
        currentStatus: ''
      }
    },
    created: function() {
      fetch(__dirname + 'src/static/cards.json')
        .then(
          (response) => {
            if (response.status !== 200) {
              console.log('Looks like there was a problem. Status Code: ' +
                response.status); 
              return;
            }

            // Examine the text in the response
            response.json().then((data) => {
              this.cards = data.media;
            });
          }
        )
        .catch((err) => {
          console.log('Fetch Error :-S', err);
        });
    },
    methods: {
      handleClick(newTab) {
        this.currentTab = newTab;
      },
      updateStatus(updatedCard) {
        let uCard = updatedCard.card;
        let uStatus = updatedCard.status;

        this.currentStatus = uStatus;

        for(let i = 0; i < this.cards.length; i++) {
          if(this.cards[i].id === uCard.id) {
            this.cards[i].status = uStatus;
          }
        }
      }
    },
    computed: {
      socialComputed() {
        if(this.socialName !== '') {
          return this.cards.filter((card) => {
            if(this.socialName.toLowerCase() === card.social_network.toLowerCase() && card.status === '') {
              return card;
            }
          });
        } else {
          return this.cards.filter((card) => {
            if(card.status === '') {
              return card;
            }
          });
        }
      },
      updatedApproved(){
        if(this.socialName !== '') {
          return this.cards.filter((card) => {
            if(this.socialName.toLowerCase() === card.social_network.toLowerCase() && card.status === 'approve') {
              return card;
            }
          });
        } else {
          return this.cards.filter((card) => {
            if(card.status === 'approve') {
              return card;
            }
          });
        }
      },
      updatedRejected(){
        if(this.socialName !== '') {
          return this.cards.filter((card) => {
            if(this.socialName.toLowerCase() === card.social_network.toLowerCase() && card.status === 'reject') {
              return card;
            }
          });
        } else {
          return this.cards.filter((card) => {
            if(card.status === 'reject') {
              return card;
            }
          });
        }
      }
    }
  }
</script>

<style lang="scss">

  * {
    box-sizing: border-box;
    font-family: sans-serif;
  }
  #app {
    width: 100%;
    position: relative;
  }

  .social-links-navigation {
    display: flex;
    flex-flow: row nowrap;
    justify-content: flex-start;
    margin: 0 0 50px;

    button {
      display: inline-block;
      width: 100%;
      max-width: 200px;
      padding: 12px 0 ;
      margin: 0;
      border: none;
      border-radius: 4px;
      background-color: #FAFAFA;
      box-shadow: 0 3px 7px rgba(0,0,0,0.3);
      text-transform: uppercase;
      text-align: center;
      text-decoration: none;
      color: #000000;

      &.active {
        background-color: #448AFF;
        color: #ffffff;
      }
    }
  }

  .default-tabs {
  position: relative;
  margin: 0 auto;
  &__item {
    display: inline-block;
    margin: 0 5px;
    padding: 10px;
    padding-bottom: 8px;
    font-size: 16px;
    letter-spacing: 0.8px;
    color: gray;
    text-decoration: none;
    border: none;
    background-color: transparent;
    border-bottom: 2px solid transparent;
    cursor: pointer;
    transition: all 0.25s;
    &_active {
      color: black;
    }
    &:hover {
      border-bottom: 2px solid gray;
      color: black;
    }
    &:focus {
      outline: none;
      border-bottom: 2px solid gray;
      color: black;
    }
    &:first-child {
      margin-left: 0;
    }
    &:last-child {
      margin-right: 0;
    }
  }
  &__active-line {
    position: absolute;
    bottom: 0;
    left: 0;
    height: 2px;
    background-color: black;
    transition: transform 0.4s ease, width 0.4s ease;
  }
}
.content {
  margin-top: 30px;
  font-size: 20px;
}

// ***

.content-tab {
  width: 100%;
  position: relative;
  display: flex;
  flex-flow: row wrap;
  align-items: stretch;
  justify-content: flex-start;
}
</style>