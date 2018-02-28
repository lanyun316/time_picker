<style lang="css">
._vuec .com-calendar {
    position: absolute;
    top: 0;
    left: 0;
    z-index: 100;
    width: 100%;
    height: 100%;
    display: block;
    transition: all .5s;
    /*transform: translate(0,100%);*/
}

._vuec .scroller-component {
    display: block;
    position: relative;
    height: 170px;
    overflow: hidden;
    width: 100%;
}

._vuec .scroller-content {
    position: absolute;
    left: 0;
    top: 0;
    width: 100%;
    z-index: -1;
}

._vuec .scroller-mask {
    position: absolute;
    left: 0;
    top: 0;
    height: 100%;
    margin: 0 auto;
    width: 100%;
    z-index: 3;
    background-image: linear-gradient(to bottom, rgba(255, 255, 255, 0.95), rgba(255, 255, 255, 0.6)),
    linear-gradient(to top, rgba(255, 255, 255, 0.95), rgba(255, 255, 255, 0.6));
    background-position: top, bottom;
    background-size: 100% 68px;
    background-repeat: no-repeat;
}

._vuec .scroller-item {
    text-align: center;
    font-size: 16px;
    height: 34px;
    line-height: 34px;
    color: #000;
}

._vuec .scroller-indicator {
    width: 100%;
    height: 34px;
    position: absolute;
    left: 0;
    top: 68px;
    z-index: 3;
    background-image: linear-gradient(to bottom, #d0d0d0, #d0d0d0, transparent, transparent),
    linear-gradient(to top, #d0d0d0, #d0d0d0, transparent, transparent);
    background-position: top, bottom;
    background-size: 100% 1px;
    background-repeat: no-repeat;
}

._vuec .dp-container {
    position: fixed;
    width: 100%;
    left: 0;
    bottom: 0;
    z-index: 10000;
    background-color: #fff;
    transition: transform 0.3s ease;
    transform: translateY(0);
}

._vuec .dp-mask {
    position: fixed;
    width: 100%;
    height: 100%;
    left: 0px;
    top: 0px;
    opacity: 0;
    transition: opacity 0.1s ease;
    background-color: #000;
    z-index: 9999;
}

._vuec .dp-header {
    display: flex;
    width: 100%;
    box-align: center;
    align-items: center;
    background-image: linear-gradient(to bottom, #e7e7e7, #e7e7e7, transparent, transparent);
    background-position: bottom;
    background-size: 100% 1px;
    background-repeat: no-repeat;
    font-size: 16px;
    color: #333333;
    font-family: 'PingFang SC';
}

._vuec .dp-header .dp-item {
    color: #666;
    font-size: 18px;
    height: 44px;
    line-height: 44px;
    cursor: pointer;
}

._vuec .dp-content {
/*    display: flex;*/
    width: 100%;
    box-align: center;
    align-items: center;
    padding: 10px 0;
    font-size: 0px;
}

._vuec .dp-header .dp-item,
._vuec .dp-content .dp-item {
    box-sizing: border-box;
   /* flex: 1;*/
    text-align: center;
    display: inline-block;
    width:33.33%;
}



/*#app1{
        position:absolute;
        height: 320px;
        width:100%;
        bottom: 0px;
        
        }*/

._vuec .dp-left {
    float: left;
    width: 50%;
    padding: 10px 20px;/*px*/
}

._vuec .dp-right {
    float: right;
    width: 50%;
    padding: 10px 20px;/*px*/
    text-align: right;
}
._vuec .scroller-mask-layer{
	background-color: black;
    position: fixed;
    width: 100%;
    height: 100%;
    top: 0px;
    left: 0px;
    right: 0px;
    bottom: 0px;
    filter: alpha(opacity=50);
    -moz-opacity: 0.5;
    opacity: 0.5;
    z-index: 2;
}
._vuec .day-wheel{
        position: absolute;
        overflow: visible;
        height: 34px;
        font-size:16px;
        top:68px;
        left: 0;
        right: 0;
/*        color:$unchecked-date;*/
        -webkit-transform-style: preserve-3d;
        transform-style: preserve-3d;
   }

 ._vuec .wheel-div{
        height:34px;
        line-height: 34px;
        position: absolute;
        top:0;
        width:100%;
        text-align:center;
        -webkit-backface-visibility: hidden;
        backface-visibility:hidden;
        white-space:nowrap;
        overflow:hidden;
        text-overflow:ellipsis;
 }
</style>
<template lang="html">
	<div class="_vuec">
    	<div class="com-calendar "   v-show="onshow"  >
	    	<div class="dp-container">
	    		<div class="dp-header">
	    			<div class=" dp-left"  @click = "canceldata" v-text="cancelWord">取消</div>
                	<div class=" dp-right"  @click = "choiceDate" v-text="okWord">确定</div>  			
	    		</div>	   		
		   	    <div class="dp-content"> 	   		            
		            <div class="dp-item" v-if="plate.indexOf('YYYY')>-1" v-bind:style="{width:percent+'%'}">
		              	<data-scroll :datatype="'year'" :datalist="datalistyear" :defaultdata="fouceyear" v-on:focus-data="getfoucedata"></data-scroll>
		            </div>
		            <div class="dp-item" v-if="plate.indexOf('MM')>-1" v-bind:style="{width:percent+'%'}">
		            	<data-scroll :datatype="'mouth'" :datalist="datalistmouth" :defaultdata="foucemouth" v-on:focus-data="getfoucedata"></data-scroll>
		            </div>
		            <div class="dp-item" v-if="plate.indexOf('DD')>-1" v-bind:style="{width:percent+'%'}">
		            	<data-scroll :datatype="'day'" :datalist="datalistday" :defaultdata="fouceday" v-on:focus-data="getfoucedata"></data-scroll>
		            </div>
		            <div class="dp-item" v-if="plate.indexOf('HH')>-1" v-bind:style="{width:percent+'%'}">
		            	<data-scroll :datatype="'hour'" :datalist="datalisthour" :defaultdata="foucehour" v-on:focus-data="getfoucedata"></data-scroll>
		            </div>
		            <div class="dp-item" v-if="plate.indexOf('mm')>-1" v-bind:style="{width:percent+'%'}">
		            	<data-scroll :datatype="'minute'" :datalist="datalistminute" :defaultdata="fouceminute" v-on:focus-data="getfoucedata"></data-scroll>
		            </div>
		            <div class="dp-item" v-if="plate.indexOf('ss')>-1" v-bind:style="{width:percent+'%'}">
		            	<data-scroll :datatype="'second'" :datalist="datalistsecond" :defaultdata="foucesecond" v-on:focus-data="getfoucedata"></data-scroll>
		            </div>
	             </div>
             </div>
	 	</div>
	</div>
</template>
<script lang="js">
		'use strict';
	       var vm;
		module.exports ={
	    	data:function(){
	    	return {
	    		datalistyear:["1970","1971"],
	    		datalistmouth:['1月','2月','3月','4月','5月','6月','7月','8月','9月','10月','11月','12月'],
	    		datalistday:['1','2','3','4','5','6','7','8','9','10','11','12','13','14','15','16','17','18','19','20','21','22','23','24','25','26','27','28','29','30'],
	    		datalisthour:["1","2","3","4","5","6","7","8","9","10","11","12","13","14","15","16","17","18","19","20","21","22","23","24"],
	    		datalistminute:["1","2","3","4","5","6","7","8","9","10","11","12","13","14","15","16","17","18","19","20","21","22","23","24","25","26","27","28","29","30","31","32","33","34","35","36","37","38","39","40","41","42","43","44","45","46","47","48","49","50","51","52","53","54","55","56","57","58","59","60"],
	    		datalistsecond:["1","2","3","4","5","6","7","8","9","10","11","12","13","14","15","16","17","18","19","20","21","22","23","24","25","26","27","28","29","30","31","32","33","34","35","36","37","38","39","40","41","42","43","44","45","46","47","48","49","50","51","52","53","54","55","56","57","58","59","60"],
	    		sYear:this.startyear!=undefined?this.startyear:1920,
	    		eYear:this.endyear!=this.endyear?this.endyear:2050,
	    		sMouth:1,
	    		eMouth:12,
	    		sDay:1,
	    		eDay:31,
	    		fouceyear:"2016",
	    		foucemouth:"4",
	    		fouceday:"23",
	    		foucehour:"7",
	    		fouceminute:"1",
	    		foucesecond:"1",
	    		percent:"100",
	    		plate:this.layout!=undefined?this.layout:"YYYY-MM-DD",
	    		format:this.form!=undefined?this.form:"YYYY-MM-DD",
	    		dTime:this.defaulttime,
	    		okWord:this.okword!=undefined?this.okword:"确定",
	    		cancelWord:this.cancelword!=undefined?this.cancelword:"取消",
	    		startTime:this.starttime,
	    		endTime:this.endtime
	    	}
	    },
	    props:{
	    	defaulttime:{
	    		type:String
	    	},
	    	startyear:{
	    		type:Number
	    	},
	    	endyear:{
	    		type:Number
	    	},
	    	starttime:{
	    		type:String
	    	},
	    	endtime:{
	    		type:String
	    	},
	    	form:{
	    		type:String
	    	},
	    	layout:{
	    		type:String
	    	},
	    	okword:{
	    		type:String
	    	},
	    	cancelword:{
	    		type:String
	    	},
	    	onshow:{
	    		type:Boolean
	    	}
	    },
	    methods:{
			getyearlist:getyearlist,
			getdefaulttime:getdefaulttime,
			getdatamouth:getdatamouth,
			getfoucedata:getfoucedata,
			getDaysInOneMonth:getDaysInOneMonth,
			getdaylist:getdaylist,
			choiceDate:choiceDate,
			getmouth:getmouth,
			resolveStime:resolveStime,
			resolveEtime:resolveEtime,
			canceldata:canceldata
	   },
	   mounted:function(){
		    vm=this;
			if(vm.startTime!=undefined){
				resolveStime();
			}
			if(vm.endTime!=undefined){
				resolveEtime();
			}
		   var sYear=vm.sYear;
		   var eYear=vm.eYear;
		   getyearlist(sYear,eYear);
		   getdefaulttime();
		   if(vm.plate=="YYYY-MM-DD HH:mm:ss"){
			   vm.percent=100/6+"";
		   }
		   if(vm.plate=="YYYY-MM-DD"){
			   vm.percent=100/3+"";
		   }
	   },
	   watch:{
		   datalistmouth:cdatalistmouth,
		   fouceyear:cfouceyear,
		   foucemouth:cfoucemouth
	   },
	   components: {
            'data-scroll':require('modules/common/components/time_picker/3d_data_scroll.vue')
       }	
	 }		 		
	 function getdefaulttime(){
		  var plate=vm.plate;
		  var defaulttime=vm.dTime;
		  if(defaulttime!=undefined){		  
				switch (plate)
				{
				 case 'YYYY-MM-DD': 
					 var defaulttimes=defaulttime.split("-");
					  vm.fouceyear=defaulttimes[0];
					  vm.foucemouth=getdatamouth(defaulttimes[1])+"月";
					  vm.fouceday=getdatamouth(defaulttimes[2]);
				    break;			
				 case 'YYYY':
					 vm.fouceyear=defaulttime+"";
				     break;
				 case 'MM': 
					 vm.foucemouth=defaulttime+"月";
				      break;
				 case 'DD': 
					 vm.fouceday=defaulttime+"";
				      break;
				 default:
					 var defaulttimes=defaulttime.split("-");
					 vm.fouceyear=defaulttimes[0];
					 vm.foucemouth=getdatamouth(defaulttimes[1])+"月";
					 vm.fouceday=getdatamouth(defaulttimes[2]);
					 var myDate = new Date();
					 vm.foucehour=myDate.getHours()+"";
					 vm.fouceminute=myDate.getMinutes()+"";
					 vm.foucesecond=myDate.getSeconds()+"";
				}		  
			  return false;
		  }
		  var myDate = new Date();
		  vm.fouceyear=myDate.getFullYear()+"";
		  vm.foucemouth=(myDate.getMonth()+1)+"月";
		  vm.fouceday=myDate.getDate()+"";
		  vm.foucehour=myDate.getHours()+"";
		  vm.fouceminute=myDate.getMinutes()+"";
		  vm.foucesecond=myDate.getSeconds()+"";
	}
	 
	function getyearlist(sYear,eYear){
		   var datalistyear=[];
		   for(var i=sYear;i<=eYear;i++){
			   datalistyear.push(i+"");
		   }
		   vm.datalistyear=datalistyear;
	}

	function getmouth(mouth){
		 if(mouth<10){
			 mouth="0"+mouth;
		 }
		 return mouth;
	}

	function getdatamouth(mouth){
		if(mouth.charAt(0)=="0"){
			mouth=mouth.split("0")[1];
		}
		return mouth;
	}

	function cdatalistmouth(){
		
	}

	function getfoucedata(data){
		var dType=data.dType;
		var focusData=data.focusData;
		this.$emit('fouce-data',data);		
		switch (dType)
		{
		 case 'year':
			 vm.fouceyear=focusData;
		     break;
		 case 'mouth': 
			 vm.foucemouth=focusData;
		      break;
		 case 'day': 
			 vm.fouceday=focusData;
		      break;
		 case 'hour': 
			 vm.foucehour=focusData;
		      break;
		 case 'minute': 
			 vm.fouceminute=focusData;
		      break;
		  default:
			  vm.foucesecond=focusData;
		}
	}

	function cfouceyear(){
		getmouthlist();
		if(parseInt(vm.foucemouth)<parseInt(vm.datalistmouth[0])||parseInt(vm.foucemouth)>parseInt(vm.datalistmouth[vm.datalistmouth.length-1])){
			  vm.foucemouth=parseInt(vm.datalistmouth[0])+"月";
		}
		getdaylist();
		if(parseInt(vm.fouceday)<parseInt(vm.datalistday[0])||parseInt(vm.fouceday)>parseInt(vm.datalistday[vm.datalistday.length-1])){
			  vm.fouceday=parseInt(vm.datalistday[0])+"";
		}
	}

	function cfoucemouth(){
		getdaylist();
		if(parseInt(vm.fouceday)<parseInt(vm.datalistday[0])||parseInt(vm.fouceday)>parseInt(vm.datalistday[vm.datalistday.length-1])){
			  vm.fouceday=parseInt(vm.datalistday[0])+"";
		}
		 
	}
	
	function getDaysInOneMonth(year, month){
		 var new_date = new Date(year,month,1);          
		 return (new Date(new_date.getTime()-1000*60*60*24)).getDate();   
	} 

	function getmouthlist(){
		var focusyear=parseInt(vm.fouceyear);
		var startyear=vm.sYear;
		var endyear=vm.eYear;
		var startmouth=1;
		var endmouth=12;
		var mouthlist=[];
		if(focusyear==startyear){
			startmouth=parseInt(vm.sMouth);
		}
		if(focusyear==endyear){
			endmouth=parseInt(vm.eMouth);
			console.log(vm.eMouth);
		}
		for(var i=startmouth;i<=endmouth;i++){
			mouthlist.push(i+"月");
		}
		vm.datalistmouth=mouthlist;
	}

	function getdaylist(){
		var focusyear=parseInt(vm.fouceyear);
		var startyear=vm.sYear;
		var endyear=vm.eYear;
		var focusmouth=parseInt(vm.foucemouth);
		var startmouth=vm.sMouth;
		var endmouth=vm.eMouth;
		var maxday=getDaysInOneMonth(focusyear,focusmouth);
		var startday=1;
	    var endday=maxday;
	    if((focusyear==startyear)&&(focusmouth==startmouth)){
	    	startday=parseInt(vm.sDay);
	    }
	    if((focusyear==endyear)&&(focusmouth==endmouth)){
	    	endday=parseInt(vm.eDay);
	    }
		var daylist=[];
		for(var i=startday;i<=endday;i++){
			daylist.push(i+"");	
		}	
		vm.datalistday=daylist;
	}
	
	function choiceDate(){
		var plate=vm.plate;
		var format=vm.format;
		switch (plate)
		{
		 case 'YYYY-MM-DD': 
			format=format.replace('YYYY',getmouth(parseInt(vm.fouceyear)));
			format=format.replace('MM', getmouth(parseInt(vm.foucemouth)));
			format=format.replace('DD', getmouth(parseInt(vm.fouceday)));
		    break;	
		 case 'YYYY':
			 format=format.replace('YYYY', vm.fouceyear);
		     break;
		 case 'MM': 
			 format=format.replace('MM', getmouth(parseInt(vm.foucemouth)));
		      break;
		 case 'DD': 
			 format=format.replace('DD', getmouth(parseInt(vm.fouceday)));
		      break;
		 default:
			format=format.replace('YYYY', vm.fouceyear);
			format=format.replace('MM', getmouth(parseInt(vm.foucemouth)));
			format=format.replace('DD', getmouth(parseInt(vm.fouceday)));
			format=format.replace('HH', getmouth(parseInt(vm.foucehour)));
			format=format.replace('mm', getmouth(parseInt(vm.fouceminute)));
			format=format.replace('ss', getmouth(parseInt(vm.foucesecond)));
		}
		this.$emit('choice-date',format);	
	}

	function resolveStime(){
		var plate=vm.plate;
		var startTime=vm.startTime;
		switch (plate)
		{
		 case 'YYYY':
			 vm.sYear=startTime;
		     break;
		 case 'MM': 
			 vm.sMouth=startTime;
		      break;
		 case 'DD': 
			 vm.sDay=startTime;
		      break;
		 default:
			var startTimes=startTime.split("-");
			vm.sYear=startTimes[0];
			vm.sMouth=startTimes[1];
			vm.sDay=startTimes[2];
		}
	}

	function resolveEtime(){
		var plate=vm.plate;
		var endTime=vm.endTime;
		switch (plate)
		{
		 case 'YYYY':
			 vm.eYear=endTime;
		     break;
		 case 'MM': 
			 vm.eMouth=endTime;
		      break;
		 case 'DD': 
			 vm.eDay=endTime;
		      break;
		 default:
			var endTimes=endTime.split("-");
			vm.eYear=endTimes[0];
			vm.eMouth=endTimes[1];
			vm.eDay=endTimes[2];
		}
	}
	
	function canceldata(){
		var data=false;
		this.$emit('cancel-date',data);	
	}
</script>