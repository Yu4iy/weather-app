<template>
    <section>
		<!-- <div class="window" @mouseover="setup">
			<div class="glass">
					<div class="display">
						<div class="face" >
							{{curentFace}}
						</div>
					</div>
					<img  class="" :src ="require(`../assets/${getimg()}.jpg`)" alt="">
					<div class="lins"></div>
					<div v-if="setup" class="interface-circle"></div>
					<div class="blink"></div>
				
					<div class="temp">
						<div class="temp__deg" v-show="temp">{{Math.round(temp)}}°C</div>
						<div class="temp__">{{status}}</div>
					</div>
			</div>
			<input  @keyup.enter="getTemp(search)" v-model="search" class="input" placeholder="Press Enter"  type="text">
		</div>
		<div class="small">
			<div class="glass">
					<div class="display"></div>
					<div class="lins"></div>
					<div v-if="setup" class="interface-circle"></div>
					<div class="blink"></div>
			</div>
		</div> -->





	<div class="window">
		<div class="glass">
			<div class="display">
				<div v-if="setup" class="display-wrapper">
					<div v-if="showImg" class="display-img" :class="{ 'cold' : temp < 0 , 'normal' : temp > 0, 'hot' : temp > 20   }"></div>
					<div class="interface-circle"></div>
						<div v-if="faceStatus" class="face">
							{{curentFace}}
						</div>
				</div>
			</div>

					<!-- OK -->
			<div class="lins"></div>
			<div class="blink"></div> 
		</div>
			<div class="togle"
			@click="setDisplay()"
			@mouseenter ="curentFace = sadFace"
			@mouseleave ="curentFace = startFace"
			>
				<div class="switch"  
				:class="{ 'active': setup}">
					<i class="fa-solid fa-power-off"></i>
				</div>
			</div>
			<span class="input" :class="{ 'input-stage-1': openInputStageOne, 'input-stage-2': openInputStageTwo}">
			<div class="input-lins"></div>
				<input  
					@keyup.enter="getTemp(search)" 
					v-model="search" 
					@click="curentFace = smileFace"

					class="" placeholder=""  type="text">
			</span>
		<div class="small-window">
			<div class="small-window-display">
					<div class="small-window-display-wraper">
							<div class="small-window-display-wraper-mesage" v-show="setup">
									<h2 >{{mesage}}</h2>
									<div>{{city}}</div>
							</div>
					</div>
			</div>
			<div v-if="setup" class="interface-circle-smal"></div>
			<div class="blink-smal"></div>
			<div class="lins-smal"></div>
		</div>
	</div>
    </section>
</template>

<script>
import axios from 'axios'
export default {
  name: 'app',
  data(){
    return{
      search:'',
		startFace:"'_'",
		sadFace:">_<",
		smileFace:'^_^',
		curentFace:'',
		temp:null,
		openInputStageOne:false,
		openInputStageTwo:false,
		faceStatus:true,
		showImg:false,
      status:false,
		mesage:'',
		setup:false,
    }
  },
  mounted () {

  },
  methods:{
    getTemp(elem){

    if(elem != ''){
      axios
     .get(`http://api.openweathermap.org/data/2.5/weather?q=${elem}&lang=it&units=metric&appid=8498a805d4666a2c1dd118f2dd52154e`)
     .then(response => {
			console.log(response.data);
			this.temp = response.data.main.temp
         this.search = '';	
			this.city = response.data.name + ',' + response.data.sys.country;  
			this.mesage = response.data.main.temp + '°C';
			this.curentFace = ''
			this.showImg = true
			this.faceStatus = false
       })
    } 
    },
	setMesage(){
		const mesages = ['Hi,I am Weather Bot ','Insert name of City', 'And press Enter']
		mesages.forEach((element,index) => {
			setTimeout(() => 
			this.mesage = element
			,3000 * index);
		});
	},
	setDisplay(){
		if(this.status == false){
			this.curentFace = this.startFace
			this.status = true
			this.setup = true
			setTimeout(this.setMesage, 0)
			setTimeout(() => 
			this.openInputStageTwo = true
			,2000);
			setTimeout(() => 
			this.openInputStageOne = true
			,1000);
			
		}
		else if(this.status == true){
			this.status = false
			this.setup = false
			this.mesage = ''
			clearTimeout(this.setMesage); 
			setTimeout(() => 
			this.openInputStageOne = false
			,1000);
			this.openInputStageTwo = false
			this.curentFace = this.face3
         this.search = '';	
			this.showImg = false
			this.faceStatus = true
			this.city = ''

		}

	},
  },
}

</script>

<style scoped lang="scss">

    section{
      box-sizing: border-box;
      display: flex;
      align-items: center;
      justify-content: center;
      height: 100vh;
      font-family: sans-serif;

	}



.window{
position: relative;
padding: 30px;
width: 400px; height: 400px;
border-radius: 50%;
background:   
repeating-radial-gradient(
      rgba(#e4e4e4, 0) 0, 
      rgba(#e4e4e4, 0) 23px, 
      rgba(#e4e4e4, .05) 25px, 
      rgba(#e4e4e4, 0) 27px) content-box, 
repeating-radial-gradient(
      rgba(#a6a6a6, 0) 0, 
      rgba(#a6a6a6, 0) 13px, 
      rgba(#a6a6a6, .05) 15px, 
      rgba(#a6a6a6, 0) 17px) content-box, 
repeating-radial-gradient(
      rgba(#8b8b8b, 0) 0, 
      rgba(#8b8b8b, 0) 19px, 
      rgba(#8b8b8b, .05) 21px, 
      rgba(#8b8b8b, 0) 23px) content-box, 

conic-gradient(
      #cdcdcd, #9d9d9d, #808080, 
      #bcbcbc, #c4c4c4, #e6e6e6, 
      #dddddd, #a1a1a1, #7f7f7f, 
      #8b8b8b, #bfbfbf, #e3e3e3, 
      #d2d2d2, #a6a6a6, #858585, 
      #8d8d8d, #c0c0c0, #e5e5e5, 
      #d6d6d6, #9e9e9e, #828282, 
      #8f8f8f, #bdbdbd, #e3e3e3, #cdcdcd) ;

box-shadow: 
		0 -1px 1px #eee, 
		0 2px 2px #1d1d1d, 
		inset 0 0 1px #666, 
		inset 0 1px .125em #8b8b8b, 
		inset 0 2px .25em #a4a2a3, 
		inset 0 -1px .125em #8b8b8b, 
		inset 0 -2px .25em #a4a2a3, 
		inset 0 0 0 .375em #cdcdcd;

			&:hover .display-hover{
			color: #fff;
			text-transform: uppercase;
			text-shadow:
			0 0 7px #fff,
			0 0 10px #fff,
			0 0 21px #fff,
			0 0 32px #0fa,
			0 0 42px #0fa,
		}
		.togle{
			width: 90px;
			height: 40px;
			position: absolute;
			bottom: -60px;
			left: 50%;
			transform: translate(-50%);
			
			box-shadow: 
					0 -1px 1px #eee, 
					0 2px 2px #1d1d1d, 
					inset 0 0 1px #666, 
					inset 0 1px .125em #8b8b8b, 
					inset 0 2px .25em #a4a2a3, 
					inset 0 -1px .125em #8b8b8b, 
					inset 0 -2px .25em #a4a2a3, 
					inset 0 0 0 .375em #cdcdcd;
			background:
					radial-gradient(black 15%, transparent 16%) 0 0,
					radial-gradient(black 15%, transparent 16%) 8px 8px,
					radial-gradient(rgba(255,255,255,.1) 15%, transparent 20%) 0 1px,
					radial-gradient(rgba(255,255,255,.1) 15%, transparent 20%) 8px 9px;
					background-color:#282828;
					background-size:16px 16px;
					overflow: hidden;
					border-radius: 5px;		
				display: flex;
				align-items: center;
				cursor: pointer;
			.switch{
				width: 35px;
				height: 20px;
				margin: 0 10px;
			transition: linear 0.2s;

				background:   
				conic-gradient(
						#cdcdcd, #c7c7c7, #d3d2d2, 
						#bcbcbc, #c4c4c4, #e6e6e6, 
						#dddddd, #cccbcb, #bdbbbb, 
						#d2d2d2, #c9c9c9, #bbbbbb, 
						#c4c3c3, #c0c0c0, #e5e5e5, 
						#d6d6d6, #cccccc, #d1d1d1, 
						#d1d1d1, #bdbdbd, #e3e3e3, #cdcdcd) ;
				box-shadow: 
					0 -1px 1px rgb(129, 129, 129), 
					inset 0 0 1px #666, 
					inset 0 1px .125em #8b8b8b, 
					inset 0 2px .25em #a4a2a3, 
					inset 0 -1px .125em #8b8b8b, 
					inset 0 0 0 .375em #cdcdcd;
				border-radius: 3px;		
				display: flex;
				align-items: center;
				justify-content: center;
				}
			.active {
				transform: translatex(35px);
				color: rgb(255, 255, 255);
				text-shadow:
					0 0 7px #fff,
					0 0 8px #fff,
					0 0 15px #fff,
					0 0 21px #fff,
					0 0 32px #0fa,
					0 0 62px #0fa,
					0 0 82px #0fa,
			}
		}
		
	} 
	

// DISPALY 
.display{
	position: absolute;
	top: 50%;
	left: 50%;
	transform: translate(-50%, -50%);
	width: 97.8%;
	height: 97.8%;
	border-radius: 100%;
	background:
	radial-gradient(black 15%, transparent 16%) 0 0,
	radial-gradient(black 15%, transparent 16%) 8px 8px,
	radial-gradient(rgba(255,255,255,.1) 15%, transparent 20%) 0 1px,
	radial-gradient(rgba(255,255,255,.1) 15%, transparent 20%) 8px 9px;
	background-color:#282828;
	background-size:16px 16px;
	overflow: hidden;
	





// WRAPPER
	.display-wrapper{
		display: flex;
		justify-content: center;
		align-items: center;
		position: relative;
		width: 100%;
		height: 100%;

		.interface-circle{

				width: 90%;
				height: 90%;
				border-radius: 100%;
				overflow: hidden;
				border: 5px solid rgba(255, 255, 255, 0.13);	
				border-bottom-style: dashed;
				border-top-style: dashed;
				animation: rotate 25s linear infinite forwards;
			}
			@keyframes rotate {
			0%   {transform: rotate(0deg);}
			100% {transform: rotate(360deg);}
			}
				.face{
		position: absolute;
		top: 50%;
		left: 50%;
		font-weight: 900;
		transform: translate(-50%, -50%);
		font-size: 140px;
		color: #fff;
		text-transform: uppercase;
		text-shadow:
			0 0 7px #fff,
			0 0 10px #fff,
			0 0 21px #fff,
			0 0 32px #0fa,
			0 0 42px #0fa,
			0 0 62px #0fa,
			0 0 82px #0fa,
			0 0 121px #0fa;
	}
	.display-img{
				position: absolute;
				width: 100%;
				height: 100%;
				border-radius: 100%;
				overflow: hidden;
	}
	}
}
	.cold{
		background-image: url('../assets/8.jpg');
		background-size: cover;
		background-position: center bottom;
}
	.normal{
		background-image: url('../assets/9.jpg');
		background-size: cover;
		width: 100%	;
		background-position: right bottom  -100px;
}
	.hot{
		background-image: url('../assets/2.jpg');
		background-size: cover;
		background-position: right bottom  -50px;
}

// GLASS IMAGE
.glass{
	position: relative;
	width: 100%;
	height: 100%;
	border-radius: 100%;
	overflow: hidden;
	img{
		position: absolute ; 
		width: 400px;
		bottom: 0px;
		
	}
	.lins{
	position: absolute;
	top: 50%;
	left: 50%;
	transform: translate(-50%, -50%);
	width: 97.8%;
	height: 97.8%;
	background: rgba(244, 239, 239, 0.212);
	border-radius: 10px;
	border: 8px solid rgba( 255, 255, 255, 0.18 );
	border-radius: 100%;
	overflow: hidden;
}
.blink{
	position: absolute;
	left: -140px;
	top: -60px;
	transform: rotate(140deg);
	width: 500px;
	height: 210px;
	background: rgba(255, 255, 255, 0.082);
	}
}
// INPUT 
.input{
			position: absolute;
			bottom: 65%;
			left: 139%;
			transform: translateX(-50%);
			border-radius: 10px;
			width: 9%;
			height: 35px;
			transition: linear 1s;
			display: flex;
			justify-content: center;
			align-items: center;
			background:
			radial-gradient(black 15%, transparent 16%) 0 0,
			radial-gradient(black 15%, transparent 16%) 8px 8px,
			radial-gradient(rgba(255,255,255,.1) 15%, transparent 20%) 0 1px,
			radial-gradient(rgba(255,255,255,.1) 15%, transparent 20%) 8px 9px;
			background-color:#282828;
			background-size:16px 16px;

			

			.input-lins{

			}

			input{
				width: 99%;
				height: 99%;
				background: rgba(255, 255, 255, 0.267);
				border-radius: 10px;
				outline: none;
				text-align: center;
				overflow: hidden;
												box-shadow: 
										0 -1px 1px #eee, 
										0 2px 2px #1d1d1d, 
										inset 0 0 1px #666, 
										inset 0 1px .125em #8b8b8b, 
										inset 0 2px .25em #a4a2a3, 
										inset 0 -1px .125em #8b8b8b, 
										inset 0 -2px .25em #a4a2a3, 
										inset 0 0 0 .375em #cdcdcd;
										z-index: -1;
						z-index: 1;

			color: #fff;
text-transform: uppercase;
letter-spacing: 3px;
text-shadow:
	0 0 10px #fff,
	0 0 21px #fff,
	0 0 32px #0fa,
	0 0 42px #0fa;
	
			}
					&::after{
						content: '';
						position: absolute;
						width: 10px;
						height: 40px;
						left: 50%;
						top: -40px;
						transform: translatex(-50%);

						background:   
								repeating-radial-gradient(
										rgba(#e4e4e4, 0) 0, 
										rgba(#e4e4e4, 0) 23px, 
										rgba(#e4e4e4, .05) 25px, 
										rgba(#e4e4e4, 0) 27px) content-box, 
								repeating-radial-gradient(
										rgba(#a6a6a6, 0) 0, 
										rgba(#a6a6a6, 0) 13px, 
										rgba(#a6a6a6, .05) 15px, 
										rgba(#a6a6a6, 0) 17px) content-box, 
								repeating-radial-gradient(
										rgba(#8b8b8b, 0) 0, 
										rgba(#8b8b8b, 0) 19px, 
										rgba(#8b8b8b, .05) 21px, 
										rgba(#8b8b8b, 0) 23px) content-box, 

								conic-gradient(
										#cdcdcd, #9d9d9d, #808080, 
										#bcbcbc, #c4c4c4, #e6e6e6, 
										#dddddd, #a1a1a1, #7f7f7f, 
										#8b8b8b, #bfbfbf, #e3e3e3, 
										#d2d2d2, #a6a6a6, #858585, 
										#8d8d8d, #c0c0c0, #e5e5e5, 
										#d6d6d6, #9e9e9e, #828282, 
										#8f8f8f, #bdbdbd, #e3e3e3, #cdcdcd) ;

								box-shadow: 
										0 -1px 1px #eee, 
										0 2px 2px #1d1d1d, 
										inset 0 0 1px #666, 
										inset 0 1px .125em #8b8b8b, 
										inset 0 2px .25em #a4a2a3, 
										inset 0 -1px .125em #8b8b8b, 
										inset 0 -2px .25em #a4a2a3, 
										inset 0 0 0 .375em #cdcdcd;
										z-index: -1;
		}

		
		

  }
	.input-stage-1{
	bottom: 45%;
  }
	.input-stage-2{
		width: 50%;
	}



// SMALLWINDOW 

.small-window{
	position: absolute; 
	top: -25%;
	left: 105%;
	padding: 30px;
	width: 250px; height: 250px;
	border-radius: 50%;
	overflow: hidden;
	display: flex;
	justify-content: center;
	align-items: center;
	background:   
repeating-radial-gradient(
      rgba(#e4e4e4, 0) 0, 
      rgba(#e4e4e4, 0) 23px, 
      rgba(#e4e4e4, .05) 25px, 
      rgba(#e4e4e4, 0) 27px) content-box, 
repeating-radial-gradient(
      rgba(#a6a6a6, 0) 0, 
      rgba(#a6a6a6, 0) 13px, 
      rgba(#a6a6a6, .05) 15px, 
      rgba(#a6a6a6, 0) 17px) content-box, 
repeating-radial-gradient(
      rgba(#8b8b8b, 0) 0, 
      rgba(#8b8b8b, 0) 19px, 
      rgba(#8b8b8b, .05) 21px, 
      rgba(#8b8b8b, 0) 23px) content-box, 

conic-gradient(
      #cdcdcd, #9d9d9d, #808080, 
      #bcbcbc, #c4c4c4, #e6e6e6, 
      #dddddd, #a1a1a1, #7f7f7f, 
      #8b8b8b, #bfbfbf, #e3e3e3, 
      #d2d2d2, #a6a6a6, #858585, 
      #8d8d8d, #c0c0c0, #e5e5e5, 
      #d6d6d6, #9e9e9e, #828282, 
      #8f8f8f, #bdbdbd, #e3e3e3, #cdcdcd) ;

box-shadow: 
		0 -1px 1px #eee, 
		0 2px 2px #1d1d1d, 
		inset 0 0 1px #666, 
		inset 0 1px .125em #8b8b8b, 
		inset 0 2px .25em #a4a2a3, 
		inset 0 -1px .125em #8b8b8b, 
		inset 0 -2px .25em #a4a2a3, 
		inset 0 0 0 .375em #cdcdcd;

	.small-window-display{
	position: absolute;
	top: 50%;
	left: 50%;
	transform: translate(-50%, -50%);
	width: 90%;
	height: 90%;
	border-radius: 100%;
	background:
	radial-gradient(black 15%, transparent 16%) 0 0,
	radial-gradient(black 15%, transparent 16%) 8px 8px,
	radial-gradient(rgba(255,255,255,.1) 15%, transparent 20%) 0 1px,
	radial-gradient(rgba(255,255,255,.1) 15%, transparent 20%) 8px 9px;
	background-color:#282828;
	background-size:16px 16px;
	overflow: hidden;
	}
	.blink-smal{
	position: absolute;
	left: -140px;
	top: -120px;
	transform: rotate(130deg);
	width: 500px;
	height: 210px;
	background: rgba(255, 255, 255, 0.082);
	}
	.lins-smal{
	position: absolute;
	top: 50%;
	left: 50%;
	transform: translate(-50%, -50%);
	width: 97.8%;
	height: 97.8%;
	background: rgba(244, 239, 239, 0.212);
	border-radius: 10px;
	border: 8px solid rgba( 255, 255, 255, 0.18 );
	border-radius: 100%;
	overflow: hidden;
}
.interface-circle-smal{
				width: 95%;
				height: 95%;
				border-radius: 100%;
				overflow: hidden;
				border: 5px solid rgba(255, 255, 255, 0.13);	
				border-bottom-style: dashed;
				border-top-style: dashed;
				animation: rotate-smal 25s linear infinite forwards;
			}
			@keyframes rotate-smal {
			0%   {transform: rotate(-20deg);}
			100% {transform: rotate(-340deg);}
			
}
}


.small-window-display-wraper{
position: relative;
height: 100%;
display: flex;
justify-content: center;
}
.small-window-display-wraper-data{
height: 100%;
display: flex;
justify-content: center;
align-items: center;
flex-direction: column;


color: #fff;
text-transform: uppercase;
text-shadow:
	0 0 10px #fff,
	0 0 21px #fff,
	0 0 32px #0fa,
	0 0 42px #0fa,
	0 0 62px #0fa,
	0 0 82px #0fa,
	0 0 121px #0fa;


h1{
	margin: 0;
	padding: 0;
	font-size: 45px;
	margin: 0 0 10px 0;
}
div{

}
}
.small-window-display-wraper-mesage{
height: 100%;
font-size: 25px;
text-align: center;
display: flex;
justify-content: center;
align-items: center;
flex-direction: column;
max-width: 240px;
color: #fff;
text-transform: uppercase;
text-shadow:
	0 0 10px #fff,
	0 0 21px #fff,
	0 0 32px #0fa,
	0 0 42px #0fa,
	0 0 62px #0fa,
	0 0 82px #0fa,
	0 0 92px #0fa;
h2{
	margin: 6px;
max-width: 220px;

	}
}

</style>
