<template>
  <div class="convos-notifications is-sidebar">
    <div class="row">
      <div class="col s12">
        <h5>
          <a href="#mark-as-read" v-tooltip="unreadTooltip()" @click.prevent="markAsRead()" class="btn-floating waves-effect waves-light" :class="markAsReadClass()"><i class="material-icons">check</i></a>
          Notifications
        </h5>
      </div>
    </div>
    <div class="row notification read" v-if="!user.notifications.length">
      <div class="col s12">
        <h5>convosbot</h5>
        <div class="message">No notifications.</div>
      </div>
    </div>
    <div class="row notification" :class="$index >= user.unread ? 'read' : ''" @click="gotoMessage(msg)" v-for="msg in user.notifications">
      <div class="col s12">
        <span class="secondary-content ts" v-tooltip="msg.ts.toLocaleString()">{{msg.ts | timestring}}</span>
        <h5>{{msg.from}} in {{msg.dialog_id}}</h5>
        <div class="message">{{msg.message}}</div>
      </div>
    </div>
  </div>
</template>
<script>
module.exports = {
  props: ["user"],
  methods: {
    gotoMessage: function(msg) {
      // TODO: Should also jump to the correct time
      this.settings.main = "#chat/" + msg.connection_id + "/" + msg.dialog_id;
    },
    markAsRead: function() {
      var self = this;
      Convos.api.readNotifications({}, function(err, xhr) {
        if (!err) self.user.unread = 0;
      });
    },
    markAsReadClass: function() {
      return this.user.unread ? '' : 'disabled';
    },
    unreadTooltip: function() {
      return this.user.unread
        ? 'Mark ' + this.user.unread + ' notifications as read.'
        : 'All notifications are read.';
    }
  }
};
</script>
