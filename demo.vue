
<template lang="html">
<div id="app">	
	 <time-picker    :defaulttime="defaulttime"  :layout="'YYYY-MM-DD'" :form="'YYYY-MM-DD'"   :onshow="onshow"    v-on:choice-date="onOk"  v-on:choice-show="onChange" v-on:cancel-date="onshowf">		
	 </time-picker>
	<input type="text" v-model="timeValue" @click="putshow">	
</div>
</template>

<script lang="js">
	module.exports = {
		data: function() {
			return {
				defaulttime: '2011-02-14',
				syear: 1970,
				eyear: 2020,
				okwords: 'ok',
				cancelwords: 'cancel',
				timeValue: "",
				onshow: false
			}
		},
		mounted: mounted,
		methods: {
			onOk: function(data) {
				var vm = this;
				vm.timeValue = data;
				vm.onshow = false;
			},
			onChange: function(data) {
				console.log(data);
			},
			putshow: function() {
				this.onshow = true;
			},
			onshowf: function(data) {
				console.log(data);
				var vm = this;
				vm.onshow = false;
			}
		},
		components: {
			'time-picker':require('modules/common/components/time_picker/time_picker.vue')
		}
	}

	function mounted() {
		var vm = this;
	}
</script>