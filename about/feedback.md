
<script type="text/javascript" src="@/root.animation.js"></script>
<script type="text/javascript" src="@/root.textarea.js"></script>
<script type="text/javascript" src="@/root.input.js"></script>

# 问题反馈

关于开源工具和网站的任何问题或对系统功能的任何建议和意见都可以向我们反馈，您的热心和关注是我们不断前进的动力。

-- 10 --

反馈内容

<textarea id="Feedback" style="width: 94%" required rows="10"></textarea>

电子邮件 &nbsp; /gray,14:方便的话可以留下您的电子邮件，处理完成后将通过邮件反馈。网站不会发送任何广告邮件。/

<input type="email" id="Email" maxlength="255" style="width: 92%" value+="{ $storage.get('email') ?? '' }" />

-- 20 --

<div class="center"><button debug watch="#Feedback" scale="normal" hint="#Message" color="prime" id="FeedbackButton" message-duration="20" failure-text="您已经提交过，不需要重复提交。" success-text="反馈已经提交，我们会尽快处理。" exception-text="出错了：{data}" enable-on-success="false" enable-on-failure="false" onclick+="post:/api/system/feedback?guid={ $storage.get('guid') }&email=$(#Email)%&feedback=$(#Feedback)%#/data -> not-zero"> &nbsp; 提交反馈 &nbsp; </button></div>
<div id="Message" class="f14 center l250"></div>

-- 50 --