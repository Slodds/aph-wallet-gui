<template>
  <section id="dashboard--contacts">
    <div class="header">
      <h1 class="underlined">Contacts</h1>
    </div>
    <div class="body">
      <section id="contacts--table">
        <div class="body">
          <div v-for="(contact, index) in filteredContacts" :key="index"
             @click="useContact(contact)" class="contact">
            <div class="cell name">{{ contact.name }}</div>
            <div class="cell copy">
              <aph-copy-text :text="contact.address"></aph-copy-text>
            </div>
          </div>
        </div>
      </section>
    </div>
  </section>
</template>

<script>
export default {
  computed: {
    filteredContacts() {
      const searchBy = this.searchBy.toLowerCase();
      const list = _.filter(this.$services.contacts.getAllAsArray(), ({ name, address }) => {
        if (!name || !address) {
          return false;
        }

        return name.toLowerCase().indexOf(searchBy) > -1
          || address.toLowerCase().indexOf(searchBy) > -1;
      });
      return list;
    },
  },

  data() {
    return {
      searchBy: '',
    };
  },

  methods: {
    useContact(contact) {
      // todo, send this over to the address field on the send form automatically?
      console.log(contact);
    },
  },
};
</script>

<style lang="scss">
#dashboard--contacts {
  @extend %tile-light;

  display: flex;
  flex-direction: column;
  padding-bottom: $space-lg;

  .header {
    padding: $space-lg;

    h1.underlined {
      @extend %underlined-header;

      flex: 1;
      margin-bottom: 0;
    }
  }

  .body {
    padding: 0 $space-lg;
    overflow: auto;

    #contacts--table {
      .header {
        display: flex;
        padding: 0;

        .cell {
          @extend %small-uppercase-grey-label;

          flex: 1;
          padding: $space;
        }
      }

      .body {
        height: 90%;
        overflow-y: auto;
        padding: 0;

        .contact {
          align-items: center;
          background: transparent;
          border-top: 1px solid $background;
          display: flex;
          transition: $transition;
          flex-wrap: wrap;

          .cell {
            flex: 1;
            font-family: GilroySemibold;
            padding: $space;

            &.copy {
              flex: none;
            }
          }

          &:hover, &.active {
            background: $background;
          }

        }
      }
    }
  }



}
</style>

