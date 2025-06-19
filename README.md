// 适用于 Auto.js v4.1.1 或 Auto.js Pro
auto.waitFor();

toast("启动TikTok...");
app.launchApp("TikTok");
sleep(5000);

// 尝试点击“我”或“Profile”
if (text("我").exists()) {
    text("我").findOne().click();
} else if (text("Profile").exists()) {
    text("Profile").findOne().click();
} else {
    toast("找不到个人主页按钮");
    exit();
}
sleep(3000);

// 尝试点击“关注”或“Following”
if (text("关注").exists()) {
    text("关注").findOne().click();
} else if (text("Following").exists()) {
    text("Following").findOne().click();
} else {
    toast("找不到关注列表");
    exit();
}
sleep(4000);

// 开始取关循环
let maxCount = 30; // 最多操作30人，防封控
let count = 0;

while (count < maxCount) {
    let btn = textMatches(/(已关注|Following)/).findOne(3000);
    if (btn) {
        btn.click();
        sleep(1000);
        // 检查是否有确认弹窗
        let confirmBtn = textMatches(/(取消关注|Unfollow)/).findOne(2000);
        if (confirmBtn) {
            confirmBtn.click();
        }
        count++;
        toast("已取消关注第 " + count + " 人");
        sleep(2000);
    } else {
        toast("找不到更多按钮");
        break;
    }
    // 向下滑动一点继续
    swipe(500, 1600, 500, 1000, 600);
    sleep(1500);
}

toast("自动取关完成，共处理 " + count + " 人");
