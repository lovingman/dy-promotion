<template>
	<div class="page-container profile">
		<!-- 头像 -->
		<van-row :gutter="15" type="flex" justify="space-between" align="center">
		  <van-col>
		  	<van-image round width="60" height="60" :src="avatarUrl" />
		  </van-col>
		  <van-col span="14">
		  	<div class="fs_18 font-weight-normal">{{name}}</div>
		  	<div class="fs_12 font-weight-normal mt-2">手机号：<span>{{tel}}</span></div>
		  </van-col>
		  <van-col class="text-center">
		  	<van-icon name="qr" size="36"/>
		  	<p class="m-0">关注公众号</p>
		  </van-col>
		</van-row>

		<!-- 邀请码 -->
		<section class="mt-4 bg_dark-400 member p-3">
			<van-row :gutter="15">
			  <van-col :span="12">
			  	<div class="d-flex justify-content-between align-items-center">
			  		<div class="fs_12 font-weight-normal text-999 w-70 text-truncate">微信：<span>{{weChatId}}</span></div>
			  		<van-button plain hairline size="mini">编辑</van-button>
			  	</div>
			  </van-col>
			  <van-col :span="12">
			  	<!-- 是会员 -->
			  	<div class="d-flex justify-content-between align-items-center" v-if="isMember">
			  		<div class="fs_12 font-weight-normal text-999 w-70 text-truncate">邀请码：<span>{{invitationCode}}</span></div>
			  		<van-button plain hairline size="mini" 
			  			v-clipboard:copy="invitationCode"
        			v-clipboard:success="onCopy"
        			v-clipboard:error="onError">复制</van-button>
			  	</div>
			  	<!-- 未开通会员 -->
			  	<div class="d-flex justify-content-between align-items-center" v-else>
			  		<div class="fs_12 font-weight-normal text-999">未开通会员</div>
			  		<van-button plain hairline size="mini" @click="toBeMember">开通</van-button>
			  	</div>
			  </van-col>
			</van-row>
		</section>

		<!-- 提现 -->
		<section class="mt-4 bg_white withdraw">
			<div class="d-flex justify-content-between align-items-center p-3 withdraw_up">
				<div>
					<h4 class="fs_12 m-0">可提现余额</h4>
					<p class="m-0 fs_26 mt-2">{{withdraw.overage}}</p>
				</div>
				<router-link :to="'myPurse'">
					<van-button color="#FE2C58">我的钱包</van-button>
				</router-link>
			</div>
			<van-row :gutter="15" class="p-3">
			  <van-col :span="7" class="text-center">
			  	<span class="fs_14 font-weight-normal text-999">总收益(元)</span>
			  	<p class="fs_20 m-0 mt-2">{{withdraw.total}}</p>
			  </van-col>
			  <van-col :span="10" class="text-center">
			  	<span class="fs_14 font-weight-normal text-999">今日预估收益(元)</span>
			  	<p class="fs_20 m-0 mt-2">{{withdraw.today}}</p>
			  </van-col>
			  <van-col :span="7" class="text-center">
			  	<span class="fs_14 font-weight-normal text-999">昨日收益(元)</span>
			  	<p class="fs_20 m-0  mt-2">{{withdraw.yesterday}}</p>
			  </van-col>
			</van-row>
		</section>

		<!-- 我的团队 -->
		<section class="mt-4">
			<h3 class="fs_18 m-0">我的团队</h3>
			<div class="mt-3 bg_dark-400 official">
				<div class="d-flex justify-content-between align-items-center p-3">
					<div class="d-flex align-items-center">
						<van-image round width="45" height="45" :src="directorUrl" />
						<div class="ml-3">
							<h4 class="fs_18 m-0">{{directorWechat}}</h4>
							<p class="m-0 fs_12 mt-1 opacity-40">遇到问题加微信咨询我哦</p>
						</div>
					</div>
					<van-button color="#FE2C58" 
						v-clipboard:copy="directorWechat"
        		v-clipboard:success="onCopy"
        		v-clipboard:error="onError">复制微信</van-button>
				</div>
			</div>

			<div class="mt-3 bg_dark-400 recruit p-3">
				<van-row>
					<van-col span="6" class="text-center">
						<router-link :to="'myTeam'">	
							<van-image width="35" height="35" :src="require('@/assets/images/recruit_team.png')" />
							<p class="m-0 mt-1 font-weight-normal">我的团队</p>
						</router-link>
					</van-col>
					<van-col span="6" class="text-center">
						<router-link :to="'poster'">	
							<van-image width="35" height="35" :src="require('@/assets/images/recruit_pic.png')" />
							<p class="m-0 mt-1 font-weight-normal">招商素材</p>
						</router-link>
					</van-col>
					<van-col span="6" class="text-center">
						<router-link :to="''">	
							<van-image width="35" height="35" :src="require('@/assets/images/recruit_service.png')" />
							<p class="m-0 mt-1 font-weight-normal">在线客服</p>
						</router-link>
					</van-col>
					<van-col span="6" class="text-center">
						<router-link :to="''"
							v-clipboard:copy="teamWechat"
        			v-clipboard:success="onCopy"
        			v-clipboard:error="onError">	
							<van-image width="35" height="35" :src="require('@/assets/images/recruit_group.png')" />
							<p class="m-0 mt-1 font-weight-normal">加入群聊</p>
						</router-link>
					</van-col>
				</van-row>
				<!-- 是会员 -->
				<div class="mt-3" v-if="isMember">
					<van-button class="border-radius-4 fs_16" size="large">点击这里招募团队</van-button>
				</div>
				<!-- 未开通会员 -->
				<div class="mt-3" v-else>
					<van-row :gutter="15">
						<van-col span="12">
							<van-button class="border-radius-4 fs_16 invite_bth" size="large">邀请好友</van-button>
						</van-col>
						<van-col span="12">
							<van-button class="border-radius-4 fs_16" size="large" @click="toBeMember">开通会员</van-button>
						</van-col>
					</van-row>
					
				</div>
			</div>
		</section>

		<!-- 其他功能 -->
		<section class="mt-4">
			<h3 class="fs_18 m-0">其他功能</h3>
			<van-row class="mt-3">
				<van-col span="6" class="text-center mb-3">
					<router-link :to="'douyin'">
						<van-image width="35" height="35" :src="require('@/assets/images/other_fun1.png')" />
						<p class="m-0 mt-2 font-weight-normal">我的抖音号</p>
					</router-link>
				</van-col>
				<van-col span="6" class="text-center mb-3">
					<router-link :to="'income'">
						<van-image width="35" height="35" :src="require('@/assets/images/other_fun2.png')" />
						<p class="m-0 mt-2 font-weight-normal">收益报表</p>
					</router-link>
				</van-col>
				<van-col span="6" class="text-center mb-3">
					<router-link :to="'data'">
						<van-image width="35" height="35" :src="require('@/assets/images/other_fun3.png')" />
						<p class="m-0 mt-2 font-weight-normal">服务协议</p>
					</router-link>
				</van-col>
				<van-col span="6" class="text-center mb-3">
					<router-link :to="'data'">
						<van-image width="35" height="35" :src="require('@/assets/images/other_fun4.png')" />
						<p class="m-0 mt-2 font-weight-normal">隐私政策</p>
					</router-link>
				</van-col>
				<van-col span="6" class="text-center mb-3">
					<router-link :to="'data'">
						<van-image width="35" height="35" :src="require('@/assets/images/other_fun5.png')" />
						<p class="m-0 mt-2 font-weight-normal">推广规范</p>
					</router-link>
				</van-col>
			</van-row>
		</section>
	</div>
</template>

<script>
	export default {
		name: 'index',
		data () {
			return {
				isMember:false, // 是否开通会员
				avatarUrl:"https://img.yzcdn.cn/vant/cat.jpeg",
				name: "片刻安静",
				tel:"13687986540",
				weChatId:"higrbjjt",
				invitationCode:"WFS557DD",
				withdraw:{
					overage:"0.00",
					total:"15.00",
					today:"0.00",
					yesterday:"12.00"
				},
				directorUrl:"https://img.yzcdn.cn/vant/apple-2.jpg",
				directorWechat:"负责人微信",
				teamWechat:"微信群的ID"
			}
		},
		components: {},
		methods:{
			// 复制到粘贴板成功
			onCopy: function (e) {
	      this.$toast.success("复制成功\n" + e.text);
	    },
	    // 复制到粘贴板失败
	    onError: function (e) {
	      this.$toast.fail("复制失败");
	    },
	    // 开通会员
	    toBeMember(){
	    	this.$router.push("openMember");
	    },

		},
	}
</script>

<style>

</style>