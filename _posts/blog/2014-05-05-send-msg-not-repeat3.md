---
layout: post
title: [Lua]从池子里抽出一条消息，不能与前3次抽到的消息重复
description: 从池子里抽出一条消息，不能与前3次抽到的消息重复
category: blog
---


##从池子里抽出一条消息，不能与前3次抽到的消息重复
```lua

MaxRepeat = 3;
MsgTable = {
  "这是一条消息",
  "这是一条消息",
  "这是一条消息",
  "这是一条消息",
}

--方法一
LastSendAnnSuffixTable = LastSendAnnSuffixTable or {};
function GetSuffix()
    local msgCount = #MsgTable;
    local suffix;
    if msgCount > MaxRepeat then
        --循环直至找到不予前MaxRepeat个重复的下标
        while true do
            local n = math.random(1, msgCount);
            local isRecent = false
            for k,v in pairs(LastSendAnnSuffixTable) do
                if v == n then
                    isRecent = true;
                end
            end

            if not isRecent then
                suffix = n;
                break 
            end
        end
        --修改最近三个下标
        LastSendAnnSuffixTable[3] = LastSendAnnSuffixTable[2];
        LastSendAnnSuffixTable[2] = LastSendAnnSuffixTable[1];
        LastSendAnnSuffixTable[1] = suffix;
    else
        suffix = math.random(1, msgCount);
    end

    return suffix;
end


--方法二
LastDomain = nil;
function GetSuffix2()
    local msgCount =  #MsgTable;
    local suffix;

    -- n条不重复处理：划分为 msgCount/n 个区间 任意两个区间内
    -- 的两条消息不重复，抽取每个区间的概率均等。
    if msgCount > MaxRepeat then 
        LastDomain = LastDomain or (math.floor( msgCount / 2  / MaxRepeat));
        MaxDonmain = math.floor(msgCount / MaxRepeat);

        local pos;
        if LastDomain == 0 then
            pos = 0.0;
        elseif LastDomain == MaxDonmain then
            pos = 1.0;
        else
            pos = 0.5;
        end
        local isLeftRange = math.random() < pos;

        local n;
        if isLeftRange then
            n = math.random(1, LastDomain * MaxRepeat - 1);
        else
            n = math.random(LastDomain * MaxRepeat + MaxRepeat, msgCount);
        end

        LastDomain = math.floor(n / MaxRepeat);
        suffix = n;
    else
        suffix = math.random(1, msgCount);
    end

    return suffix;
end
```
