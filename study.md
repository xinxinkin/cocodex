使用CocoStudio
UI Editor：
先把项目导出的json和资源文件放到TestGame项目的Resource目录中
 
1. 在HelloWorldScene.cpp顶部添加引用
#include "cocos-ext.h"
using namespace cocos2d::extension;
USING_NS_CC;
 
2. 创建一个UILayer层
UILayer* ul = UILayer::create();
this->addChild(ul);
ul->addWidget(CCUIHELPER->createWidgetFromJsonFile("文件名.json"));
 
3. 绑定UILayer层上控件的事件
tbOk = dynamic_cast<UITextButton*>(ul->getWidgetByName("tbOk"));
tbOk->addReleaseEvent(this, coco_releaseselector(HelloWorld::tbOkCallback));
 
增加回调函数：
void HelloWorld::tbOkCallback(cocos2d::CCObject *pSender) {
//获取文本框值，并打印
UITextField* tfOldPwd = dynamic_cast<UITextField*>(ul->getWidgetByName("tfOldPwd"));
CCLog(tfOldPwd->getStringValue());
}
 
Animation Editor：
先把项目导出的json和资源文件放到TestGame项目的Resource目录中
 
1. 文件头添加引用(同上)
 
2. 使用示例：

// 首先读取png,plist和ExportJson/json文件
CCArmatureDataManager::sharedArmatureDataManager()->addArmatureFileInfo("ActionEditor/Cowboy0.png", "ActionEditor/Cowboy0.plist", "ActionEditor/Cowboy.ExportJson");

//然后创建armature类，并将进行初始化
CCArmature *armature = CCArmature::create("Cowboy");

//然后选择播放动画0，并进行缩放和位置设置
armature->getAnimation()->playByIndex(0);

//该模板中共制作了三个动画，你可以将索引修改为0/1/2中的任意值来查看不同效果
armature->setScale(0.5f);
armature->setPosition(ccp(visibleSize.width * 0.5, visibleSize.height * 0.5));

//最后将armature添加到场景中
this->addChild(armature,2);
 
Scene Editor：
先把项目导出的json和资源文件放到TestGame项目的Resource目录中
 
1. 文件头添加引用(同上)
2. 使用示例：
找到void HelloWorld::menuCloseCallback(CCObject* pSender)这个方法，并输入下面代码：
 
//创建一个新的场景
CCScene* newscene = CCScene::create();

//从CocoStudio场景编辑器生成的数据生成根节点
CCNode* pNode = CCJsonReader::sharedJsonReader()->createNodeWithJsonFile("SceneEditorTest/SceneEditorTest.json");
 
//播放背景音乐
CCComAudio* pAudio = (CCComAudio*)(pNode->getComponent("Audio"));
pAudio->playBackgroundMusic(pAudio->getFile(), pAudio->getIsLoop());

//给蝴蝶鱼配置动画
CCComRender* pFishRender = (CCComRender*)(pNode->getChildByTag(10010)->getComponent( "butterFlyFish"));
CCArmature* pButterFlyFish= (CCArmature*)(pFishRender->getRender());
pButterFlyFish->getAnimation()->playByIndex(0);
newscene->addChild(pNode, 0, 1);

//切换到新的场景
CCDirector::sharedDirector()->replaceScene(newscene);
 
3. 场景切换效果：
CCScene *s = SecondPage::scene();
CCDirector::sharedDirector()->setDepthTest(true);
CCTransitionScene *transition = CCTransitionPageTurn::create(3.0f, s, false);
