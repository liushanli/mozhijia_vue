<template>
	<view style="padding: 0.3rem 1rem 0;">
		<rich-text  :nodes="content|formatRichText"></rich-text>
	</view>
</template>

<script>
	export default {
			data() {
				return {
					content:""
				}
			},
			filters: {
					/**
					 * 处理富文本里的图片宽度自适应
					 * 1.去掉img标签里的style、width、height属性
					 * 2.img标签添加style属性：max-width:100%;height:auto
					 * 3.修改所有style里的width属性为max-width:100%
					 * 4.去掉<br/>标签
					 * @param html
					 * @returns {void|string|*}
					 */
					formatRichText(html) {
			            if(!html) return
						//控制小程序中图片大小
						let newContent = html.replace(/<img[^>]*>/gi, function(match, capture) {
							match = match.replace(/style="[^"]+"/gi, '').replace(/style='[^']+'/gi, '');
							match = match.replace(/width="[^"]+"/gi, '').replace(/width='[^']+'/gi, '');
							match = match.replace(/height="[^"]+"/gi, '').replace(/height='[^']+'/gi, '');
							return match;
						});
						newContent = newContent.replace(/style="[^"]+"/gi, function(match, capture) {
							match = match.replace(/width:[^;]+;/gi, 'max-width:100%;').replace(/width:[^;]+;/gi, 'max-width:100%;');
							return match;
						});
						newContent = newContent.replace(/<br[^>]*\/>/gi, '');
						newContent = newContent.replace(/\<img/gi, '<img style="max-width:100%;height:auto;display:inline-block;margin:10rpx auto;"');
						return newContent;
					}
				},
			onLoad() {
				this.content = uni.getStorageSync("shopIntroduce");
			}
			
		}
</script>

<style>
</style>
