<template>
<div class="chat-panel panel panel-default">
    <div class="panel-heading">
        <i class="fa fa-comments fa-fw"></i> 師生互動專區
        <div class="btn-group pull-right">
            <button type="button" class="btn btn-default btn-xs dropdown-toggle" data-toggle="dropdown">
                <i class="fa fa-chevron-down"></i>
            </button>
            <ul class="dropdown-menu slidedown">
                <li>
                    <a href="#">
                        <i class="fa fa-refresh fa-fw"></i> Refresh
                    </a>
                </li>
                <li>
                    <a href="#">
                        <i class="fa fa-check-circle fa-fw"></i> Available
                    </a>
                </li>
                <li>
                    <a href="#">
                        <i class="fa fa-times fa-fw"></i> Busy
                    </a>
                </li>
                <li>
                    <a href="#">
                        <i class="fa fa-clock-o fa-fw"></i> Away
                    </a>
                </li>
                <li class="divider"></li>
                <li>
                    <a @click="signout">
                        <i class="fa fa-sign-out fa-fw"></i> Sign Out
                    </a>
                </li>
            </ul>
        </div>
    </div>
    <!-- /.panel-heading -->
    <div class="panel-body">
        <ul class="chat">
            <li class="left clearfix" v-for="item in items"  v-bind:key="item.id" v-if="item.user==$session.get('user')">
                <span class="chat-img pull-left">
                    <img src="http://placehold.it/50/55C1E7/fff" alt="User Avatar" class="img-circle" />
                </span>
                <div class="chat-body clearfix">
                    <div class="header">
                        <strong class="primary-font">{{ item.user }}</strong>
                        <small class="pull-right text-muted">
                            <i class="fa fa-clock-o fa-fw"></i> {{ item.dtime }}
                        </small>
                    </div>
                    <p>
                        {{ item.content }}
                    </p>
                </div>
            </li> 
            <li class="right clearfix" v-else>
                <span class="chat-img pull-right">
                    <img src="http://placehold.it/50/FA6F57/fff" alt="User Avatar" class="img-circle" />
                </span>
                <div class="chat-body clearfix">
                    <div class="header">
                        <small class=" text-muted">
                            <i class="fa fa-clock-o fa-fw"></i> {{ item.dtime }}</small>
                        <strong class="pull-right primary-font">{{ item.user }}</strong>
                    </div>
                    <p>
                        {{ item.content }}
                    </p>
                </div>
            </li>
        </ul>
    </div>
    <!-- /.panel-body -->
    <div class="panel-footer">
        <div class="input-group">
            <input id="btn-input" ref="message" v-on:keyup.enter="sendmsg({'user': $session.get('user'), 'content': $refs.message.value})" type="text" class="form-control input-sm" placeholder="想說點什麼...?" />
            <span class="input-group-btn">
                <button @click="sendmsg({'user': $session.get('user'), 'content': $refs.message.value})" class="btn btn-warning btn-sm" id="btn-chat">
                    送出
                </button>   
            </span>
        </div>                          
    </div>    
</div>

</template>
<script>
import { mapGetters, mapActions } from 'vuex'
export default {
  mounted () {
    this.$session.start()
    if (!this.$session.has('user')) {
      this.$session.set('user', prompt('Please enter your name', ''))
    }
  },
  computed: {
    ...mapGetters({
      message: 'message',
      items: 'items'
    })
  },
  watch: {
    items: function (val) {
      this.$refs.message.value = null
    }
  },
  methods: {
    ...mapActions([
      'sendmsg',
      'signout'
    ])
  }
}
</script>