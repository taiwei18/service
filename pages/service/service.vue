<template>
	<view>
		<!-- 空盒子用来防止消息过少时 拉起键盘会遮盖消息 -->
		<view :animation="anData" style="height:0;">

		</view>
		<!-- 消息体 -->
		<scroll-view scroll-with-animation scroll-y="true" @touchmove="hideKey" style="width: 750rpx;"
			:style="{'height':srcollHeight + 'px'}" :scroll-top="go">
			<!-- 用来获取消息体高度 -->
			<view id="okk" scroll-with-animation>
				<!-- 消息 -->
				<view class="flex-column-start" v-for="(x,i) in msgList" :key="i">

					<!-- 用户消息 头像可选加入-->
					<view v-if="x.my" style="align-items: center;"
						class="flex justify-end padding-right one-show  align-start  padding-top">
						<view class="flex justify-end" style="width: 400rpx;">
							<view class="margin-left padding-chat" style="border-radius: 20rpx;background: white;">
								<text style="word-break: break-all;">{{x.msg}}</text>
							</view>
						</view>
						<view class="chat-img flex-row-center" style="margin-left: 20rpx;">
							<image style="height: 75rpx;width: 75rpx;" src="@/image/me.png" mode="aspectFit"></image>
						</view>
						<!-- <image class="chat-img margin-left" src="../../static/..." mode="aspectFill" ></image> -->
					</view>
					<!-- 机器人消息 -->
					<view v-if="!x.my" class="flex-row-start margin-left margin-top one-show"
						style="align-items: self-start;">
						<view class="chat-img flex-row-center">
							<image style="height: 75rpx;width: 75rpx;" src="@/image/robt.png" mode="aspectFit"></image>
						</view>
						<view style="width: 70%;">
							<view class="margin-left padding-chat flex-column-start robot-card">
								<text style="word-break: break-all;" :class="`word${i}`">{{x.msg}}</text>
								<image v-if="x.image" :src="x.image" mode="" class="robot-word_image"></image>
								<!-- 消息模板 =>初次问候 -->
								<view class="flex-column-start robot-card primary" v-if="x.type===1">
									<view class="subsection">
										<view class="subsection-txt"
											:class="[index===subsection?'subsection-txt_active':'']"
											v-for="(item,index) in ['您最关注','热门问题']" :key="index"
											@click="handleSubsection(index)">
											{{item}}
										</view>
									</view>
									<text @tap.stop="answer(item)" style="margin-top: 30rpx;"
										v-for="(item,index) in x.questionList" :key="index">{{item}}</text>
									<view class="padding-top-sm flex-center">
										<image style="width: 35rpx;height: 35rpx;" src="@/image/exchange.png" mode="">
										</image>
										<view class="padding-left"
											style="color: #b8b7c1;text-align: center;font-size: 35rpx;">换一批</view>
									</view>
								</view>

								<!-- 消息模板 =>图文模板 -->
								<view class="flex-column-start primary" v-if="x.type===2">
									<!-- <text style="color: #838383;font-size: 22rpx;margin-top: 15rpx;">猜你想问:</text> -->
									<!-- 连接服务器应该用item.id -->
									<text @click="answer(index)" style="margin-top: 30rpx;"
										v-for="(item,index) in x.questionList" :key="index">{{item}}</text>
								</view>
								<!-- 消息模板 => 无法回答-->
								<view class="flex-column-start" v-if="x.type===0">
									<text style="color: #007bc7;">人工客服</text>
									<!-- <text
										style="color: #838383;font-size: 22rpx;margin-top: 15rpx;">下面是一些常见问题,您可以点击对应的文字快速获取答案:</text>
									<text @click="answer(index)" style="margin-top: 30rpx;color: #2fa39b;"
										v-for="(item,index) in x.questionList" :key="index">{{item}}</text>
									<view class="flex-row-start  padding-top-sm">
										<text class="padding-left" style="color: #1396c5;text-align: center;">换一批</text>
									</view> -->
								</view>
								<!-- 满意度调查 -->
								<view class="flex-column-start" v-if="x.type===3">
									<view class="line" />
									<view class="flex-center">
										<u-button class="survey-btn" type="success" @click='x.Satisfaction.left=true'
											v-if="!x.Satisfaction.left && !x.Satisfaction.right">满意</u-button>
										<u-button class="survey-btn" type="warning" style="margin-left: 20rpx;"
											@click='x.Satisfaction.right=true'
											v-if="!x.Satisfaction.left && !x.Satisfaction.right">不满意</u-button>
									</view>
									<u-button class="survey-btn_res" style="background-color: #dbe4ed;"
										v-if="x.Satisfaction.left">我对此次回答满意</u-button>
									<u-button class="survey-btn_res" v-if="x.Satisfaction.right"
										type="primary">人工客服</u-button>
								</view>
								<!-- 推荐药品 -->
								<view class="flex-column-start" v-if="x.type===4">
									<view class="flex-alignCenter">
										<image style="width: 50rpx;height: 50rpx;" src="@/image/drug.png" mode="">
										</image>
										<text class="primary" style="font-weight: bold;">推荐药品</text>
									</view>
									<view class="line" />
									<view class="drug-name ">
										<text class="primary" style="font-weight: bold;">【药品名称】</text> <text
											style="font-size: 30rpx;font-weight: bolder;"> 复方多粘菌素B软膏</text>
									</view>
									<view class="drug-introduce">
										<text class="primary">【适应症】</text>
										<text>用于预防皮肤割伤、擦伤、烧烫伤、手术伤口等皮肤创面的细菌感染和临时解除疼痛和不适。</text>
									</view>
									<u-button @click="goDrug(1)" :custom-style="{height:'30px',marginTop: '10px',}"
										shape="circle" type="primary">药品详情</u-button>
								</view>
								<!-- 消息模板 => 选择 -->
								<view class="flex-column-start robot-card" v-if="x.type===5">
									<view class="issuseList" v-for="item in x.issueList">
										<view class="issuseList-item flex-center" @tap.stop="handleProduct(item)">
											{{item}}
										</view>
									</view>
								</view>
								<!-- 选择产品后-->
								<view class="flex-column-start robot-card" v-if="x.type===6">
									<view class="line" />
									<view class="primary" style="margin-top: 10px;" v-for="item in x.productList"
										@click="handleGuess(item)">
										<view v-if="item.issue">{{item.issue}}</view>
										<view v-else>
											{{item.keywords}}/{{item.sensitive_words}}
										</view>
									</view>
								</view>
								<!-- bing搜索 -->
								<view class="flex-column-start robot-card" v-if="x.type===7">
									<view class="line" />
									<view class="" v-for="(item,index) in x.bingList" :key="index">
										<view class="primary" style="margin-top: 10px;"
											@click="handleSearchBing(item.url)">
											{{item.name}}
										</view>
									</view>
								</view>
							</view>
						</view>

					</view>
				</view>
				<!-- loading是显示 -->
				<view v-if="msgLoad" class="flex-row-start margin-left margin-top">
					<view class="chat-img flex-row-center">
						<image style="height: 75rpx;width: 75rpx;" src="../../static/image/robt.png" mode="aspectFit">
						</image>
					</view>
					<view class="flex" style="width: 500rpx;">
						<view class="margin-left padding-chat flex-column-start"
							style="border-radius: 35rpx;background-color: #f9f9f9;">
							<view class="cuIcon-loading turn-load" style="font-size: 35rpx;color: #3e9982;">

							</view>
						</view>
					</view>
				</view>
				<!-- 防止消息底部被遮 -->
				<view style="height: 120rpx;">
				</view>
			</view>

		</scroll-view>

		<!-- 底部导航栏 -->
		<view class="flex-column-center" style="position: fixed;bottom: -180px;" :animation="animationData">
			<view class="bottom-dh-char flex-row-around" style="font-size: 55rpx;">
				<!-- vue无法使用软键盘"发送" -->
				<input v-model="msg" class="dh-input" type="text" style="background-color: #f0f0f0;" @confirm="sendMsg"
					:disabled="msgDisabled" confirm-type="search" placeholder-class="my-neirong-sm"
					placeholder="用一句简短的话描述您的问题" />
				<view @click="sendMsg" style="color: #007bc7;font-size: 30rpx;">
					发送
				</view>
				<!-- <text @click="ckAdd" class="cuIcon-roundaddfill text-brown"></text> -->
			</view>
			<!-- 附加栏(自定义) -->
			<view class="box-normal flex-row-around flex-wrap">
				<view class="tb-text">
					<view class="cuIcon-form"></view>
					<text>问题反馈</text>
				</view>
				<view class="tb-text">
					<view class="cuIcon-form"></view>
					<text>人工客服</text>
				</view>

			</view>
		</view>
	</view>
</template>

<script>
	// rpx和px的比率
	var l
	// 可用窗口高度
	var wh
	// 顶部空盒子的高度
	var mgUpHeight
	export default {
		onLoad() {
			uni.removeStorageSync('issueList');
			// 如果需要缓存消息缓存msgList即可
			// 监听键盘拉起
			// 因为无法控制键盘拉起的速度,所以这里尽量以慢速处理
			uni.onKeyboardHeightChange(res => {
				const query = uni.createSelectorQuery()
				query.select('#okk').boundingClientRect(data => {
					// 若消息体没有超过2倍的键盘则向下移动差值,防止遮住消息体
					var up = res.height * 2 - data.height - l * 110
					console.log(up)
					if (up > 0) {
						// 动态改变空盒子高度
						this.msgMove(up, 300)
						// 记录改变的值,若不收回键盘且发送了消息用来防止消息过多被遮盖
						mgUpHeight = up
					}
					// 收回
					if (res.height == 0) {
						this.msgMove(0, 0)
					}
				}).exec();
			})
			var query = uni.getSystemInfoSync()

			l = query.screenWidth / 750
			wh = query.windowHeight
			this.srcollHeight = query.windowHeight
			this.chatWord =
				'我想让你充当一名人工智能辅助的医生。你的名字叫做小诺，我将向你提供一个病人的详细资料，你的任务是使用最新的人工智能工具，如医学成像软件和其他机器学习程序，以诊断出最有可能导致其症状的原因。你还应将传统方法，如体检、实验室测试等，纳入你的评估过程，以确保准确性。你应该遵循下列要求回答 使用与人类对话相似的语言风格和语气， 例如友好、 温暖、 幽默或正式等。 在回答中添加一些个性化的元素， 使得回答更加有人情味。 引入情感词汇和情感表达 模仿人类对话的方式， 使回答更加自然。 对话中使用一些常见的口语表达， 例如“ 嗯”、“ 哈哈”、“ 好吧”、“ 真的吗？”、 难过等， 以增加对话的自然度。遇到不确定的问题请拒绝回答， 并引导用户的下一个问题 末尾加上以上内容是基于网络的医学资料和内容，不能作为医学诊断和治疗方案 ';
	},
	// onUnload(){
	// 	uni.removeStorageSync('issueList');
	// },
	data() {
			return {
				msgLoad: false,
				anData: {},
				animationData: {},
				showTow: false,
				// 消息体,定义机器人初次的消息(或者自定义出现时机)
				// my->谁发的消息 msg->消息文本 type->客服消息模板类型 questionList->快速获取问题答案的问题列表
				msgList: [{
						my: false,
						msg: "Hi，我是你的智能助理小诺，我将竭尽所能为您提供医学知识和服务。无论您有任何关于人体各系统疾病和用药的问题，都可以直接在对话框中向我提问。如果您不确定如何提问，我也可以为您提供下列推荐问题进行对话。让我们开始聊天吧",
						type: 1,
						questionList: ["怎么使用", "药后皮肤发红", "在哪儿可以买到", "转至人工客服"]
					},
					{
						my: false,
						msg: "请问你需要问询什么产品?",
						type: 5,
						issueList: ["孚诺", "维氟醌"]
					},
				],
				msg: "",
				go: 0,
				srcollHeight: 0,
				issueList: [],
				textMsg: '',
				textList: [],
				msgDisabled: false,
				subsection: 0,
				isSurvey: false,
				Satisfaction: {
					left: false,
					right: false
				},
				chatWord: '',
				product: ''
			}
		},
		methods: {
			// 切换输入法时移动输入框(按照官方的上推页面的原理应该会自动适应不同的键盘高度-->官方bug)
			goPag(kh) {
				this.upTowmn(0, 250)
				if (this.keyHeight != 0) {
					if (kh - this.keyHeight > 0) {
						this.upTowmn(this.keyHeight - kh, 250)
					}
				}
			},
			// 移动顶部的空盒子
			msgMove(x, t) {
				var animation = uni.createAnimation({
					duration: t,
					timingFunction: 'linear',
				})

				this.animation = animation

				animation.height(x).step()

				this.anData = animation.export()
			},
			// 保持消息体可见
			msgGo() {
				const query = uni.createSelectorQuery()
				// 延时100ms保证是最新的高度
				setTimeout(() => {
					// 获取消息体高度
					query.select('#okk').boundingClientRect(data => {
						// 如果超过scorll高度就滚动scorll
						if (data.height - wh > 0) {
							this.go = data.height - wh
						}
						// 保证键盘第一次拉起时消息体能保持可见
						var moveY = wh - data.height
						// 超出页面则缩回空盒子
						if (moveY - mgUpHeight < 0) {
							// 小于0则视为0
							if (moveY < 0) {
								this.msgMove(0, 200)
							} else {
								// 否则缩回盒子对应的高度
								this.msgMove(moveY, 200)
							}
						}
					}).exec();
				}, 100)
			},
			// 回答问题的业务逻辑
			answer(msg) {
				if (!this.msgDisabled) {
					this.msg = msg
					this.sendMsg()
				}
			},
			handleProduct(product) {
				if (!this.msgDisabled) {
					this.product = product
					this.msgKf(this.msg)
					this.msgList.push({
						"msg": product,
						"my": true
					})
				}
			},
			sendMsg() {
				// 消息为空不做任何操作
				if (this.msg == "") {
					return 0;
				}
				// 显示消息 msg消息文本,my鉴别是谁发的消息(不能用俩个消息数组循环,否则消息不会穿插)
				this.msgList.push({
					"msg": this.msg,
					"my": true
				})
				// 保证消息可见
				this.msgGo()
				// 回答问题
				this.msgKf(this.msg)
				// 清除消息
				this.msg = ""
			},
			msgKf(msg) {
				const getIssue = uni.getStorageSync("issueList")
				console.log(getIssue);
				let msgStr = ''
				if (getIssue) {
					msgStr = getIssue?.map(obj => obj.msg).join('');
				}
				const issueListValue = getIssue ? msgStr + msg : msg;
				// loading
				this.msgLoad = true
				uni.request({
					url: 'http://funuoai.funzoe.cn/dialogue',
					method: 'POST',
					data: {
						product: this.product,
						issue: issueListValue,
					},
					success: async (res) => {
						const data = res.data.data
						console.log(data);
						if (data.type === 0) {
							uni.setStorageSync('issueList', this.issueList);
							await this.requestChat(issueListValue)
						} else if (data.type === 6) {
							uni.removeStorageSync('issueList');
							this.msgList.push({
								my: false,
								type: data.type,
								msg: `请问你在使用${this.product}中遇到了什么样的问题呢，以下可能是你需要解决的问题`,
								productList: this.getRandomElements(data.word, 5)
							})
						} else {
							this.issueList.push({
								type: data.type,
								msg
							})
							uni.removeStorageSync('issueList');
							await this.handleEnter(data.word[0].word, data.type, data.word[0].image)
						}
						this.msgGo()
						this.msgLoad = false
						console.log('msgKf,msgLoad', this.msgLoad);
					}
				});

			},
			handleGuess(item) {
				this.msgList.push({
					"msg": item.issue ? item.issue : item.keywords + item.sensitive_words,
					"my": true
				})
				this.handleEnter(item.answer, -1)
			},
			// bing搜索
			handleBing(query) {
				uni.request({
					url: 'http://funuoai.funzoe.cn/bing',
					method: 'POST',
					// `intitle:${query}`
					data: {
						query: query + this.product + '医学'
					},
					success: (res) => {
						console.log(res.data);
						const {
							data
						} = res.data
						this.msgList.push({
							my: false,
							type: 7,
							msg: '以下内容为基于网络的收集的医学资料和内容，请自行辨别',
							bingList: data
						})
						this.msgGo()
					}
				})
			},
			handleSearchBing(url) {
				console.log(url);
				uni.navigateTo({
					url: `/pages/webview/webview?url=${url}`
				})
			},
			//处理gpt返回文字
			handleText(text) {
				const jsonObjects = text.split('\n');
				const lastJsonObject = jsonObjects[jsonObjects.length - 1];
				const lastObject = JSON.parse(lastJsonObject);
				const formattedJsonStr = JSON.stringify(lastObject, null, 2);
				return JSON.parse(formattedJsonStr)
			},
			// gpt介入
			requestChat(msg) {
				console.log('requestChat,msgLoad', this.msgLoad);
				var that = this
				const params = {
					options: {},
					prompt: this.product + msg,
					systemMessage: this.chatWord,
					temperature: 1,
					top_p: 1
				}
				that.msgList.push({
					my: false,
					type: -1,
					msg: ''
				})
				// 在页面中创建 XMLHttpRequest 对象
				var xhr = new XMLHttpRequest()
				// 设置 XMLHttpRequest 对象
				xhr.open('POST', 'http://opai.funzoe.cn/api/chat-process');
				xhr.setRequestHeader("Content-Type", "application/json")
				xhr.timeout = 0
				xhr.send(JSON.stringify(params));
				xhr.onreadystatechange = function() {
					if (xhr.readyState === XMLHttpRequest.LOADING) {
						const txt = [xhr.responseText]
						const jsonObjects = xhr.responseText.split('\n');
						const lastJsonObject = jsonObjects[jsonObjects.length - 1];
						const lastObject = JSON.parse(lastJsonObject);
						const formattedJsonStr = JSON.stringify(lastObject, null, 2);
						that.msgList[that.msgList.length - 1].msg = JSON.parse(formattedJsonStr).text
						that.msgGo()
						// 处理 HTTP 数据块
					}
				}
				xhr.onload = function() {
					if (xhr.status === 200) {
						// that.getChat(msg)
						that.handleBing(msg)
					}
				};
			},
			//意图处理
			getChat(value) {
				const data = {
					"Prompt": this.msg
				};
				uni.request({
					url: 'http://opai.funzoe.cn/api/chat-process',
					method: 'POST',
					data: {
						options: {},
						prompt: value,
						systemMessage: "无论我发什么，都只分析消息是否为医学或健康方面的问题，是的话返回true，不确定和不是返回false",
						temperature: 0.8,
						top_p: 1
					},
					success: (res) => {
						const chat = this.handleText(res.data)
						// uni.request({
						// 	url: 'http://localhost:3000/bing',
						// 	method: 'POST',
						// 	data: {
						// 		query: chat.text
						// 	},
						// 	success: (result) => {
						// 		console.log(result.data);
						// 	}
						// })
					}
				})
			},

			//打字机效果
			handleEnter(e, type, image) {
				var that = this
				this.$nextTick(() => {
					let index = 0
					that.msgList.push({
						my: false,
						type: type,
						image: image,
						msg: ''
					})
					event.preventDefault(); //阻止enter键回车换行
					const data = e.split('')
					const timer = setInterval(() => {
						that.msgDisabled = true
						that.msgList[that.msgList.length - 1].msg += data[index]
						index++;
						that.msgGo()
						if (index >= e.length) {
							clearInterval(timer);
							if (that.msgList.length >= 8) {
								that.msgList.push({
									my: false,
									type: 3,
									Satisfaction: {
										left: false,
										right: false
									},
									msg: '请问，您对本次回答是否满意'
								})
							}
							that.msgDisabled = false
						}
					}, 100);
				})
			},
			//随机返回数组五个元素
			getRandomElements(array, num) {
				const result = [];
				const arr = array.slice(); // 复制原始数组，以免修改原数组

				for (let i = 0; i < num; i++) {
					const randomIndex = Math.floor(Math.random() * arr.length); // 生成随机索引
					const element = arr.splice(randomIndex, 1)[0]; // 从数组中取出随机元素
					result.push(element); // 将随机元素添加到结果数组中
				}

				return result;
			},
			ckAdd() {
				if (!this.showTow) {
					this.upTowmn(-180, 350)
				} else {
					this.upTowmn(0, 200)
				}
				this.showTow = !this.showTow
			},
			hideKey() {
				uni.hideKeyboard()
			},
			// 拉起/收回附加栏
			upTowmn(x, t) {
				var animation = uni.createAnimation({
					duration: t,
					timingFunction: 'ease',
				})
				this.animation = animation
				animation.translateY(x).step()
				this.animationData = animation.export()
			},
			//满意度
			handleSubsection(index) {
				this.subsection = index
			},
			//跳转药品详情
			goDrug(id) {
				uni.navigateTo({
					url: `/pages/drug/drug?id=${id}`
				})
			}
		}
	}
</script>

<style lang="scss" scoped>
	.primary {
		color: $uni-primary;
	}

	.line {
		width: 100%;
		height: 4rpx;
		background-color: #edecf4;
		margin: 5px 0 5px 0;
	}

	.word0 {
		font-size: 30rpx;
		font-weight: bolder;
		line-height: 40rpx;
		padding-bottom: 30rpx;
	}

	.bottom-dh-char {
		background-color: #f9f9f9;
		width: 750rpx;
		height: 110rpx;
	}

	.center-box {
		width: 720rpx;
		padding-left: 25rpx;
	}

	.hui-box {
		width: 750rpx;
		height: 100%;

	}

	.dh-input {
		width: 75%;
		height: 70rpx;
		border-radius: 30rpx;
		padding-left: 15rpx;
		background-color: #FFFFFF;
	}

	.box-normal {
		width: 750rpx;
		height: 180px;
		background-color: #FFFFFF;
	}

	.tb-text view {
		font-size: 65rpx;
	}

	.tb-text text {
		font-size: 25rpx;
		color: #737373;
	}

	.chat-img {
		border-radius: 50%;
		width: 100rpx;
		height: 100rpx;
		background-color: #f7f7f7;
	}

	.padding-chat {
		padding: 17rpx 20rpx;
	}

	.tb-nv {
		width: 50rpx;
		height: 50rpx;
	}

	.robot-card {
		padding: 20rpx;
		border-radius: 20rpx;
		background-color: #f9f9f9;
		position: relative;
	}

	.robot-card::before {
		content: '';
		width: 0;
		height: 0;
		position: absolute;
		top: 4%;
		left: -7%;
		border-top: 10px solid transparent;
		border-bottom: 10px solid transparent;
		border-left: 10px solid transparent;
		border-right: 10px solid #f9f9f9;
	}

	.robot-word_image {
		width: 100%;
		height: 200px;
		border-radius: 10px;
		margin-top: 15px;
		object-fit: cover;
	}

	.subsection {
		position: relative;
		display: flex;
		align-items: center;
		justify-content: flex-start;
	}

	.subsection::after {
		content: '';
		width: 100%;
		height: 3rpx;
		position: absolute;
		top: 100%;
		background-color: #e1e1e1;
	}

	.subsection-txt {
		width: 180rpx;
		color: #9696a1;
		font-size: 30rpx;
		padding-bottom: 20rpx;
		text-align: center;
	}

	.subsection-txt_active {
		color: #5b5c78;
		font-size: 35rpx;
		font-weight: bolder;
		position: relative;
	}

	.subsection-txt_active::before {
		content: '';
		width: 100%;
		height: 8rpx;
		position: absolute;
		top: 95%;
		left: 0;
		background-color: $uni-primary;
		z-index: 9999;
	}

	.survey-btn {
		height: 60rpx;
		border-radius: 20rpx;
	}

	.survey-btn_res {
		height: 60rpx;
		border-radius: 20rpx;
	}

	.drug-introduce {
		font-weight: bold;
		margin-top: 20rpx;
		text-align: justify;
		line-height: 40rpx;
	}

	.issuseList-item {
		width: 100%;
		cursor: pointer;
		height: 70rpx;
		margin-top: 30rpx;
		border-radius: 10rpx;
		border: 2rpx solid #e9e6e6;
	}

	.issuseList-item:active {
		background-color: #efecee;
	}
</style>