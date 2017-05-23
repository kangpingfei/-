# -
人物法力
private Dictionary<string, UILabel> mLabels;
private Dictionary<string,UISprite> mSprites;
UISlider[] slider;
Vector2 grid_Pos = new Vector2(-5,20);  //挂在游戏界面中的位置

protected override void OnAwake()
{
    mLabels = UI.GetUIElement<UILabel>(transform);
    mSprite = UI.GetUIElemnet<UISprite>(transform);
    slider = transform.GetComponentsInChildren<UISliser>();
    Register();
    EventManager.Dispatch(EvaentConst.ManaStartPrepare);
    
    mLabes["ManaValue"].text = "0";
}

protected override void OnStart()
{
    transform.Find("grid").localPosition = GetBottomLeft() + gridPos;  //挂在游戏界面中的位置
    slider[1].value = slider[0].value = 0f;  //初始的时候  slider长度都是0
}

private void OnDestory()
{
    UnRegister;
}

private void Register()
{
    EventManager.AddListener(EventConst.ManaGrow,ManaRefresh);
}

private void UnRegister()
{
    EventManager.RemoveListener(EventConst.ManaGrow,ManaRefresh);
}

public void ManaRefresh(object param)
{
    float blockNum = (float)(param);
    slider[0].value = (float)(blockNum / 15);  //分十五个魔法格子，每三秒涨一个大格子，每个格子又划分三个小格子，每一秒涨一个小格子
    slider[1].value = slider[0].value;  //整数时，小格子和大格子数量相同，
    mLabels["ManaRefresh"].text = blockNum.ToString();  //法力值，传过来的
    StopCoroutine("Do");
    StartCoroutine("Do");
}

IEnumerator Do()
{
    float startTime = Time.time;
    float endTime = startTime + 3;
    float lastTime = Time.time;
    float delta = 1f / 45;
    while (Time.time <= endTime > 1)
    {
        if(Time.time - lastTime > 1)
        {
            slider[1].value += delta;
            lastTime = Time.time;
        }
        yield return new WaitForEndOfFrame();
    }
}
