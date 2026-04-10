# 第七党支部党员发展时间线
<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=yes">
    <title>党员发展时间线查询系统（完整版）</title>
    <style>
        * { box-sizing: border-box; margin: 0; padding: 0; }
        body {
            font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Helvetica, Arial, sans-serif;
            background-color: #f5f5f5;
            color: #333;
            line-height: 1.5;
            padding: 20px;
        }
        .container { max-width: 1000px; margin: 0 auto; }
        .card {
            background: white;
            border-radius: 20px;
            padding: 24px;
            margin-bottom: 24px;
            box-shadow: 0 4px 12px rgba(0,0,0,0.08);
        }
        .header { text-align: center; margin-bottom: 20px; }
        .header h1 { color: #d32f2f; font-size: 28px; }
        .stage-buttons {
            display: flex;
            flex-wrap: wrap;
            gap: 12px;
            justify-content: center;
            margin-bottom: 24px;
        }
        .stage-btn {
            padding: 8px 20px;
            border: 1px solid #d32f2f;
            background: white;
            color: #d32f2f;
            border-radius: 40px;
            font-size: 16px;
            font-weight: 500;
            cursor: pointer;
            transition: all 0.2s;
        }
        .stage-btn.active { background: #d32f2f; color: white; }
        .search-area {
            display: flex;
            gap: 12px;
            margin-bottom: 20px;
        }
        .search-area input {
            flex: 1;
            padding: 12px 16px;
            border: 1px solid #ddd;
            border-radius: 40px;
            font-size: 16px;
        }
        .search-area button {
            background: #d32f2f;
            color: white;
            border: none;
            border-radius: 40px;
            padding: 0 24px;
            font-size: 16px;
            cursor: pointer;
        }
        .timeline {
            margin-top: 20px;
        }
        .timeline-section {
            margin-bottom: 30px;
            border-left: 3px solid #d32f2f;
            padding-left: 20px;
        }
        .timeline-section h3 {
            font-size: 18px;
            margin-bottom: 12px;
            color: #d32f2f;
        }
        .detail-row {
            display: flex;
            margin-bottom: 12px;
            flex-wrap: wrap;
        }
        .detail-label {
            width: 180px;
            font-weight: 600;
            color: #555;
        }
        .detail-value {
            flex: 1;
            color: #333;
            word-break: break-word;
        }
        .no-result {
            text-align: center;
            padding: 40px;
            color: #999;
        }
        .admin-switch { text-align: right; margin-bottom: 12px; }
        .btn-outline {
            background: transparent;
            border: 1px solid #d32f2f;
            color: #d32f2f;
            border-radius: 40px;
            padding: 6px 16px;
            cursor: pointer;
            font-size: 14px;
        }
        .hidden { display: none; }
        .modal {
            display: none;
            position: fixed;
            z-index: 1000;
            left: 0;
            top: 0;
            width: 100%;
            height: 100%;
            overflow: auto;
            background-color: rgba(0,0,0,0.5);
            backdrop-filter: blur(4px);
        }
        .modal-content {
            background-color: #fefefe;
            margin: 5% auto;
            padding: 24px;
            border-radius: 24px;
            width: 90%;
            max-width: 900px;
            max-height: 90%;
            overflow-y: auto;
        }
        .close { float: right; font-size: 28px; font-weight: bold; cursor: pointer; }
        .form-item { margin-bottom: 16px; }
        .form-item label { display: block; font-weight: 500; margin-bottom: 6px; }
        .form-item input, .form-item select, .form-item textarea {
            width: 100%;
            padding: 10px;
            border: 1px solid #ddd;
            border-radius: 12px;
        }
        .member-list { margin-top: 20px; }
        .member-item {
            display: flex;
            justify-content: space-between;
            align-items: center;
            padding: 12px 0;
            border-bottom: 1px solid #eee;
        }
        .edit-btn, .del-btn {
            padding: 4px 12px;
            border-radius: 20px;
            border: none;
            cursor: pointer;
            margin-left: 8px;
        }
        .edit-btn { background: #1976d2; color: white; }
        .del-btn { background: #e0e0e0; }
        .btn-primary {
            background: #d32f2f;
            color: white;
            border: none;
            border-radius: 40px;
            padding: 10px 20px;
            cursor: pointer;
        }
        .btn-secondary {
            background: #e0e0e0;
            border: none;
            border-radius: 40px;
            padding: 10px 20px;
            cursor: pointer;
        }
        .info-note {
            background: #f9f9f9;
            padding: 10px;
            border-radius: 12px;
            margin-top: 16px;
            font-size: 14px;
            color: #666;
        }
    </style>
</head>
<body>
<div class="container">
    <div class="admin-switch">
        <button id="toggleAdminBtn" class="btn-outline">🔐 管理员入口</button>
    </div>

    <!-- 用户查询面板 -->
    <div id="userPanel">
        <div class="card">
            <div class="header">
                <h1>🎖️ 党员发展时间线</h1>
                <p>选择发展阶段，输入姓名查询完整历程</p>
            </div>
            <div class="stage-buttons" id="stageButtons">
                <button data-stage="积极分子" class="stage-btn">积极分子</button>
                <button data-stage="发展对象" class="stage-btn">发展对象</button>
                <button data-stage="预备党员" class="stage-btn">预备党员</button>
                <button data-stage="正式党员" class="stage-btn active">正式党员</button>
            </div>
            <div class="search-area">
                <input type="text" id="searchName" placeholder="输入姓名，例如：毛昱烨" autocomplete="off">
                <button id="searchBtn">查询</button>
            </div>
            <div id="timelineResult"></div>
        </div>
    </div>

    <!-- 管理员后台面板 -->
    <div id="adminPanel" class="hidden">
        <div class="card">
            <div id="adminAuth" class="hidden">
                <h2>管理员验证</h2>
                <input type="password" id="adminPwd" placeholder="请输入管理密码" style="width:100%; margin:10px 0; padding:10px;">
                <button id="loginBtn" class="btn-primary">登录</button>
            </div>
            <div id="adminContent" class="hidden">
                <h2>📋 完整发展记录管理</h2>
                <form id="memberForm">
                    <input type="hidden" id="editId">
                    <div class="form-item"><label>姓名</label><input type="text" id="name" required></div>
                    <div class="form-item"><label>发展阶段</label>
                        <select id="stage">
                            <option>积极分子</option><option>发展对象</option><option>预备党员</option><option>正式党员</option>
                        </select>
                    </div>
                    <div class="form-item"><label>递交入党申请书时间</label><input type="date" id="applyDate"></div>
                    <div class="form-item"><label>谈话时间（入党申请人时期）</label><input type="date" id="talkDate"></div>
                    <div class="form-item"><label>确定积极分子时间</label><input type="date" id="activeDate"></div>
                    <div class="form-item"><label>培养联系人</label><input type="text" id="cultivator"></div>
                    <div class="form-item"><label>确定为发展对象时间</label><input type="date" id="developDate"></div>
                    <div class="form-item"><label>附件2落款日期</label><input type="date" id="attachment2"></div>
                    <div class="form-item"><label>附件3落款日期</label><input type="date" id="attachment3"></div>
                    <div class="form-item"><label>附件6落款日期（多行）</label><textarea id="attachment6" rows="2" placeholder="培养联系人意见、支委会讨论时间、基层党委备案意见等"></textarea></div>
                    <div class="form-item"><label>附件10落款日期</label><input type="date" id="attachment10"></div>
                    <div class="form-item"><label>预审意见落款</label><input type="date" id="preReviewDate"></div>
                    <div class="form-item"><label>入党介绍人</label><input type="text" id="introducer"></div>
                    <div class="form-item"><label>确定为预备党员时间（支部大会吸收）</label><input type="date" id="prePartyDate"></div>
                    <div class="form-item"><label>预备党员基层党委审批时间</label><input type="date" id="approvePartyDate"></div>
                    <div class="form-item"><label>转正基层党委审批时间</label><input type="date" id="transferDate"></div>
                    <div class="button-group">
                        <button type="submit" class="btn-primary">保存</button>
                        <button type="button" id="resetFormBtn" class="btn-secondary">重置</button>
                    </div>
                </form>
                <div id="memberList" class="member-list">加载中...</div>
            </div>
        </div>
    </div>
</div>

<script>
    // ---------- 原始数据（基于Excel，包含所有列）----------
    // 日期转换函数（支持Excel序列号和中文日期）
    function parseDate(value) {
        if (!value || value === '暂无' || value === '') return '';
        if (typeof value === 'number') {
            const utc_days = value - 25569;
            const date = new Date(utc_days * 86400000);
            return date.toISOString().split('T')[0];
        }
        let str = String(value);
        // 处理 "2024年12月16日"
        let match = str.match(/(\d{4})年(\d{1,2})月(\d{1,2})日/);
        if (match) {
            let y = match[1], m = match[2].padStart(2,'0'), d = match[3].padStart(2,'0');
            return `${y}-${m}-${d}`;
        }
        // 处理 "205年11月13日" 修正为2025年
        if (str.includes('205年')) {
            str = str.replace('205年', '2025年');
            match = str.match(/(\d{4})年(\d{1,2})月(\d{1,2})日/);
            if (match) return `${match[1]}-${match[2].padStart(2,'0')}-${match[3].padStart(2,'0')}`;
        }
        // 直接返回字符串（可能是文本说明）
        return str;
    }

    // 原始数据（从Excel逐行录入，包含所有字段）
    const rawData = [
        { name:"毛昱烨", stage:"正式党员", applyDate:44814, talkDate:44832, activeDate:44864, cultivator:"姚春萌、陈淑婷", developDate:45253, attachment2:"暂无", attachment3:"暂无", attachment6:"暂无", attachment10:"暂无", preReviewDate:"暂无", introducer:"田洪冰、钱雨晨", prePartyDate:45284, approvePartyDate:45299, transferDate:45670 },
        { name:"符式珠", stage:"正式党员", applyDate:44818, talkDate:44833, activeDate:45011, cultivator:"李光萃、王小雨", developDate:45463, attachment2:"暂无", attachment3:"暂无", attachment6:"暂无", attachment10:"暂无", preReviewDate:"暂无", introducer:"陈俊杰、宋煜哲", prePartyDate:45467, approvePartyDate:45482, transferDate:45841 },
        { name:"文景", stage:"正式党员", applyDate:45175, talkDate:45194, activeDate:45221, cultivator:"何瑜楚、杨萧", developDate:45642, attachment2:45614, attachment3:45621, attachment6:"培养联系人意见：2024年11月11日<br>支委会讨论时间：2024年11月29日<br>基层党委备案意见：2024年12月16日", attachment10:45649, preReviewDate:45650, introducer:"宋煜哲、邓妙妙", prePartyDate:45655, approvePartyDate:45670, transferDate:46034 },
        { name:"阚月含", stage:"正式党员", applyDate:44879, talkDate:44883, activeDate:45221, cultivator:"潘滟滟", developDate:45642, attachment2:45614, attachment3:45621, attachment6:"培养联系人意见：2024年11月11日<br>支委会讨论时间：2024年11月29日<br>基层党委备案意见：2024年12月16日", attachment10:45649, preReviewDate:45650, introducer:"陈俊杰、宋煜哲", prePartyDate:45655, approvePartyDate:45670, transferDate:46034 },
        { name:"武佳琳", stage:"正式党员", applyDate:44879, talkDate:44883, activeDate:45033, cultivator:"吕罗玉、樊家佳", developDate:45642, attachment2:45614, attachment3:45621, attachment6:"培养联系人意见：2024年11月11日<br>支委会讨论时间：2024年11月29日<br>基层党委备案意见：2024年12月16日", attachment10:45649, preReviewDate:45650, introducer:"毛昱烨、宋煜哲", prePartyDate:45655, approvePartyDate:45670, transferDate:46034 },
        { name:"李恩希", stage:"正式党员", applyDate:45175, talkDate:45194, activeDate:45221, cultivator:"檀雨薇、宋雨星", developDate:45642, attachment2:45614, attachment3:45621, attachment6:"培养联系人意见：2024年11月11日<br>支委会讨论时间：2024年11月29日<br>基层党委备案意见：2024年12月16日", attachment10:45649, preReviewDate:45650, introducer:"毛昱烨、陈俊杰", prePartyDate:45655, approvePartyDate:45670, transferDate:46034 },
        { name:"林裕翔", stage:"正式党员", applyDate:45189, talkDate:45219, activeDate:45223, cultivator:"余嘉杭、徐腾", developDate:45642, attachment2:45614, attachment3:45621, attachment6:"培养联系人意见：2024年11月11日<br>支委会讨论时间：2024年11月29日<br>基层党委备案意见：2024年12月16日", attachment10:45649, preReviewDate:45650, introducer:"谢俸俸、邓妙妙", prePartyDate:45655, approvePartyDate:45670, transferDate:46034 },
        { name:"鲍善美", stage:"预备党员", applyDate:45170, talkDate:45170, activeDate:45221, cultivator:"许晖、罗元汝", developDate:45642, attachment2:45791, attachment3:45792, attachment6:"培养联系人意见：2025年4月30日<br>支委会讨论时间：2025年5月15日<br>基层党委备案意见：2025年5月22日", attachment10:45815, preReviewDate:45816, introducer:"陈俊杰、宋煜哲", prePartyDate:45823, approvePartyDate:45835, transferDate:"" },
        { name:"张佳蕴", stage:"预备党员", applyDate:45175, talkDate:45191, activeDate:45400, cultivator:"宋煜哲、陈俊杰", developDate:45799, attachment2:45791, attachment3:45792, attachment6:"培养联系人意见：2025年4月30日<br>支委会讨论时间：2025年5月15日<br>基层党委备案意见：2025年5月22日", attachment10:45815, preReviewDate:45816, introducer:"陈俊杰、毛昱烨", prePartyDate:45823, approvePartyDate:45835, transferDate:"" },
        { name:"王欣鑫", stage:"预备党员", applyDate:44814, talkDate:44838, activeDate:45011, cultivator:"毛昱烨、宋煜哲", developDate:45799, attachment2:45791, attachment3:45792, attachment6:"培养联系人意见：2025年4月30日<br>支委会讨论时间：2025年5月15日<br>基层党委备案意见：2025年5月22日", attachment10:45815, preReviewDate:45816, introducer:"陈俊杰、毛昱烨", prePartyDate:45823, approvePartyDate:45835, transferDate:"" },
        { name:"杜昭怡", stage:"预备党员", applyDate:45193, talkDate:45208, activeDate:45392, cultivator:"王幸虹、杨婧一", developDate:45799, attachment2:45791, attachment3:45792, attachment6:"培养联系人意见：2025年4月30日<br>支委会讨论时间：2025年5月15日<br>基层党委备案意见：2025年5月22日", attachment10:45815, preReviewDate:45816, introducer:"毛昱烨、宋煜哲", prePartyDate:45823, approvePartyDate:45835, transferDate:"" },
        { name:"石丽媛", stage:"预备党员", applyDate:44881, talkDate:44910, activeDate:45221, cultivator:"罗芮、章梦琳", developDate:45799, attachment2:45791, attachment3:45792, attachment6:"培养联系人意见：2025年4月30日<br>支委会讨论时间：2025年5月15日<br>基层党委备案意见：2025年5月22日", attachment10:45815, preReviewDate:46008, introducer:"毛昱烨、符式珠", prePartyDate:46008, approvePartyDate:46017, transferDate:"" },
        { name:"杨静雯", stage:"预备党员", applyDate:45539, talkDate:45561, activeDate:45607, cultivator:"李昌禄、王光熙", developDate:45986, attachment2:"205年11月13日", attachment3:"205年11月14日", attachment6:"培养联系人意见：2025年11月12日<br>支委会讨论时间：2025年11月19日<br>基层党委备案意见：2025年11月25日", attachment10:46001, preReviewDate:46008, introducer:"毛昱烨、符式珠", prePartyDate:46008, approvePartyDate:46017, transferDate:"" },
        { name:"周娴红", stage:"预备党员", applyDate:45369, talkDate:45377, activeDate:45607, cultivator:"杜涵月、苏丽娜", developDate:45986, attachment2:"205年11月13日", attachment3:"205年11月14日", attachment6:"培养联系人意见：2025年11月12日<br>支委会讨论时间：2025年11月19日<br>基层党委备案意见：2025年11月25日", attachment10:46001, preReviewDate:46008, introducer:"毛昱烨、符式珠", prePartyDate:46008, approvePartyDate:46017, transferDate:"" },
        { name:"卢思澄", stage:"预备党员", applyDate:45538, talkDate:45199, activeDate:45590, cultivator:"宋煜哲、陈俊杰", developDate:45986, attachment2:"205年11月13日", attachment3:"205年11月14日", attachment6:"培养联系人意见：2025年11月12日<br>支委会讨论时间：2025年11月19日<br>基层党委备案意见：2025年11月25日", attachment10:46001, preReviewDate:46008, introducer:"毛昱烨、符式珠", prePartyDate:46008, approvePartyDate:46017, transferDate:"" },
        { name:"吴雅薇", stage:"发展对象", applyDate:44890, talkDate:44913, activeDate:45221, cultivator:"周孟杰、潘滟滟", developDate:45799, attachment2:45791, attachment3:45792, attachment6:"培养联系人意见：2025年4月30日<br>支委会讨论时间：2025年5月15日<br>基层党委备案意见：2025年5月22日", attachment10:45815, preReviewDate:"暂无", introducer:"暂无", prePartyDate:"", approvePartyDate:"", transferDate:"" },
        { name:"冯文硕", stage:"发展对象", applyDate:45181, talkDate:45193, activeDate:45400, cultivator:"宋煜哲、陈俊杰", developDate:45799, attachment2:45791, attachment3:45792, attachment6:"培养联系人意见：2025年4月30日<br>支委会讨论时间：2025年5月15日<br>基层党委备案意见：2025年5月22日", attachment10:45815, preReviewDate:"暂无", introducer:"暂无", prePartyDate:"", approvePartyDate:"", transferDate:"" },
        { name:"刘翔", stage:"积极分子", applyDate:45193, talkDate:45208, activeDate:45392, cultivator:"郭嘉伊、杨佳意", developDate:"", attachment2:"", attachment3:"", attachment6:"", attachment10:"", preReviewDate:"", introducer:"", prePartyDate:"", approvePartyDate:"", transferDate:"" },
        { name:"李子涵", stage:"积极分子", applyDate:45563, talkDate:45576, activeDate:45750, cultivator:"刘扬、郭斯宇", developDate:"", attachment2:"", attachment3:"", attachment6:"", attachment10:"", preReviewDate:"", introducer:"", prePartyDate:"", approvePartyDate:"", transferDate:"" },
        { name:"梁正妍", stage:"积极分子", applyDate:45539, talkDate:45560, activeDate:45762, cultivator:"蔡慧丹、陈青莹", developDate:"", attachment2:"", attachment3:"", attachment6:"", attachment10:"", preReviewDate:"", introducer:"", prePartyDate:"", approvePartyDate:"", transferDate:"" },
        { name:"庄家秀", stage:"积极分子", applyDate:45542, talkDate:45565, activeDate:45765, cultivator:"韩家雪、刘嘉铭", developDate:"", attachment2:"", attachment3:"", attachment6:"", attachment10:"", preReviewDate:"", introducer:"", prePartyDate:"", approvePartyDate:"", transferDate:"" },
        { name:"李诗桐", stage:"积极分子", applyDate:45905, talkDate:45561, activeDate:45955, cultivator:"毛昱烨、符式珠", developDate:"", attachment2:"", attachment3:"", attachment6:"", attachment10:"", preReviewDate:"", introducer:"", prePartyDate:"", approvePartyDate:"", transferDate:"" },
        { name:"陈绵秀", stage:"积极分子", applyDate:45716, talkDate:45749, activeDate:45955, cultivator:"毛昱烨、符式珠", developDate:"", attachment2:"", attachment3:"", attachment6:"", attachment10:"", preReviewDate:"", introducer:"", prePartyDate:"", approvePartyDate:"", transferDate:"" },
        { name:"洪莹晶", stage:"积极分子", applyDate:45905, talkDate:45926, activeDate:45955, cultivator:"毛昱烨、符式珠", developDate:"", attachment2:"", attachment3:"", attachment6:"", attachment10:"", preReviewDate:"", introducer:"", prePartyDate:"", approvePartyDate:"", transferDate:"" },
        { name:"林茵茵", stage:"积极分子", applyDate:45905, talkDate:45921, activeDate:45955, cultivator:"毛昱烨、符式珠", developDate:"", attachment2:"", attachment3:"", attachment6:"", attachment10:"", preReviewDate:"", introducer:"", prePartyDate:"", approvePartyDate:"", transferDate:"" },
        { name:"林佳莹", stage:"积极分子", applyDate:45905, talkDate:45921, activeDate:45955, cultivator:"毛昱烨、符式珠", developDate:"", attachment2:"", attachment3:"", attachment6:"", attachment10:"", preReviewDate:"", introducer:"", prePartyDate:"", approvePartyDate:"", transferDate:"" },
        { name:"周思睿", stage:"积极分子", applyDate:45905, talkDate:45942, activeDate:45955, cultivator:"毛昱烨、符式珠", developDate:"", attachment2:"", attachment3:"", attachment6:"", attachment10:"", preReviewDate:"", introducer:"", prePartyDate:"", approvePartyDate:"", transferDate:"" },
        { name:"王艳", stage:"积极分子", applyDate:45905, talkDate:45926, activeDate:45955, cultivator:"毛昱烨、符式珠", developDate:"", attachment2:"", attachment3:"", attachment6:"", attachment10:"", preReviewDate:"", introducer:"", prePartyDate:"", approvePartyDate:"", transferDate:"" },
        { name:"梁涛", stage:"积极分子", applyDate:45905, talkDate:45926, activeDate:45959, cultivator:"毛昱烨、符式珠", developDate:"", attachment2:"", attachment3:"", attachment6:"", attachment10:"", preReviewDate:"", introducer:"", prePartyDate:"", approvePartyDate:"", transferDate:"" },
        { name:"李艳艳", stage:"积极分子", applyDate:44995, talkDate:45093, activeDate:45221, cultivator:"周孟杰", developDate:"2024年12月16日（已超两年有效期）", attachment2:"", attachment3:"", attachment6:"", attachment10:"", preReviewDate:"", introducer:"", prePartyDate:"", approvePartyDate:"", transferDate:"" },
        { name:"李鑫乐", stage:"积极分子", applyDate:46028, talkDate:46038, activeDate:46120, cultivator:"李恩希、林裕翔", developDate:"", attachment2:"", attachment3:"", attachment6:"", attachment10:"", preReviewDate:"", introducer:"", prePartyDate:"", approvePartyDate:"", transferDate:"" },
        { name:"林康姿", stage:"积极分子", applyDate:45908, talkDate:45921, activeDate:46120, cultivator:"毛昱烨、符式珠", developDate:"", attachment2:"", attachment3:"", attachment6:"", attachment10:"", preReviewDate:"", introducer:"", prePartyDate:"", approvePartyDate:"", transferDate:"" },
        { name:"林佳棋", stage:"积极分子", applyDate:45908, talkDate:45921, activeDate:46120, cultivator:"杨梅、张雪劼", developDate:"", attachment2:"", attachment3:"", attachment6:"", attachment10:"", preReviewDate:"", introducer:"", prePartyDate:"", approvePartyDate:"", transferDate:"" },
        { name:"王希月", stage:"积极分子", applyDate:46028, talkDate:46038, activeDate:46120, cultivator:"毛昱烨、符式珠", developDate:"", attachment2:"", attachment3:"", attachment6:"", attachment10:"", preReviewDate:"", introducer:"", prePartyDate:"", approvePartyDate:"", transferDate:"" },
        { name:"粟婷", stage:"积极分子", applyDate:46028, talkDate:46038, activeDate:46120, cultivator:"文景、武佳琳", developDate:"", attachment2:"", attachment3:"", attachment6:"", attachment10:"", preReviewDate:"", introducer:"", prePartyDate:"", approvePartyDate:"", transferDate:"" },
        { name:"张婧妍", stage:"积极分子", applyDate:46028, talkDate:46038, activeDate:46120, cultivator:"文景、武佳琳", developDate:"", attachment2:"", attachment3:"", attachment6:"", attachment10:"", preReviewDate:"", introducer:"", prePartyDate:"", approvePartyDate:"", transferDate:"" },
        { name:"翟釨嫣", stage:"积极分子", applyDate:46028, talkDate:46038, activeDate:46120, cultivator:"阚月含、李恩希", developDate:"", attachment2:"", attachment3:"", attachment6:"", attachment10:"", preReviewDate:"", introducer:"", prePartyDate:"", approvePartyDate:"", transferDate:"" }
    ];

    // 转换所有日期字段
    function normalizeMember(m) {
        return {
            id: m.id || (m.name + '_' + Date.now() + Math.random()),
            name: m.name,
            stage: m.stage,
            applyDate: parseDate(m.applyDate),
            talkDate: parseDate(m.talkDate),
            activeDate: parseDate(m.activeDate),
            cultivator: m.cultivator || '',
            developDate: parseDate(m.developDate),
            attachment2: parseDate(m.attachment2),
            attachment3: parseDate(m.attachment3),
            attachment6: m.attachment6 || '',
            attachment10: parseDate(m.attachment10),
            preReviewDate: parseDate(m.preReviewDate),
            introducer: m.introducer || '',
            prePartyDate: parseDate(m.prePartyDate),
            approvePartyDate: parseDate(m.approvePartyDate),
            transferDate: parseDate(m.transferDate)
        };
    }

    let members = [];
    const STORAGE_KEY = 'party_members_full';

    function loadMembersFromStorage() {
        const stored = localStorage.getItem(STORAGE_KEY);
        if (stored) {
            members = JSON.parse(stored);
        } else {
            members = rawData.map((m, idx) => normalizeMember({ ...m, id: idx.toString() }));
            saveMembers();
        }
    }

    function saveMembers() {
        localStorage.setItem(STORAGE_KEY, JSON.stringify(members));
    }

    function findMemberByNameAndStage(name, stage) {
        return members.find(m => m.name === name && m.stage === stage);
    }

    // 渲染完整时间线（包含所有列）
    function renderFullTimeline(member) {
        if (!member) return '<div class="no-result">未查询到该同志信息，请检查姓名和发展阶段</div>';
        
        // 按时期分组展示
        let html = `<div style="text-align:center; margin-bottom:24px;">
                        <strong style="font-size:22px;">${escapeHtml(member.name)}</strong>
                        <span style="margin-left:12px; background:#d32f2f20; padding:4px 12px; border-radius:20px;">${member.stage}</span>
                    </div>`;

        // 入党申请人时期
        html += `<div class="timeline-section"><h3>📄 入党申请人时期</h3>`;
        html += `<div class="detail-row"><div class="detail-label">递交入党申请书时间：</div><div class="detail-value">${member.applyDate || '未记录'}</div></div>`;
        html += `<div class="detail-row"><div class="detail-label">谈话时间：</div><div class="detail-value">${member.talkDate || '未记录'}</div></div>`;
        html += `</div>`;

        // 积极分子时期
        html += `<div class="timeline-section"><h3>🌱 积极分子时期</h3>`;
        html += `<div class="detail-row"><div class="detail-label">确定积极分子时间（支委会决议）：</div><div class="detail-value">${member.activeDate || '未记录'}</div></div>`;
        html += `<div class="detail-row"><div class="detail-label">培养联系人：</div><div class="detail-value">${escapeHtml(member.cultivator) || '未记录'}</div></div>`;
        html += `</div>`;

        // 发展对象时期
        html += `<div class="timeline-section"><h3>📌 发展对象时期</h3>`;
        html += `<div class="detail-row"><div class="detail-label">确定为发展对象时间（党委备案）：</div><div class="detail-value">${member.developDate || '未记录'}</div></div>`;
        html += `<div class="detail-row"><div class="detail-label">附件2落款日期：</div><div class="detail-value">${member.attachment2 || '未记录'}</div></div>`;
        html += `<div class="detail-row"><div class="detail-label">附件3落款日期：</div><div class="detail-value">${member.attachment3 || '未记录'}</div></div>`;
        html += `<div class="detail-row"><div class="detail-label">附件6（培养联系人意见/支委会/党委备案）：</div><div class="detail-value">${member.attachment6 || '未记录'}</div></div>`;
        html += `<div class="detail-row"><div class="detail-label">附件10落款日期：</div><div class="detail-value">${member.attachment10 || '未记录'}</div></div>`;
        html += `<div class="detail-row"><div class="detail-label">预审意见落款：</div><div class="detail-value">${member.preReviewDate || '未记录'}</div></div>`;
        html += `</div>`;

        // 预备党员时期
        html += `<div class="timeline-section"><h3>🔖 预备党员时期</h3>`;
        html += `<div class="detail-row"><div class="detail-label">入党介绍人：</div><div class="detail-value">${escapeHtml(member.introducer) || '未记录'}</div></div>`;
        html += `<div class="detail-row"><div class="detail-label">确定为预备党员时间（支部大会吸收）：</div><div class="detail-value">${member.prePartyDate || '未记录'}</div></div>`;
        html += `<div class="detail-row"><div class="detail-label">预备党员基层党委审批时间：</div><div class="detail-value">${member.approvePartyDate || '未记录'}</div></div>`;
        html += `</div>`;

        // 转正时期
        html += `<div class="timeline-section"><h3>✅ 转正时期</h3>`;
        html += `<div class="detail-row"><div class="detail-label">转正基层党委审批时间：</div><div class="detail-value">${member.transferDate || '未记录'}</div></div>`;
        html += `</div>`;

        html += `<div class="info-note">注：如有信息缺失或错误，请联系党支部管理员更新。</div>`;
        return html;
    }

    function escapeHtml(str) {
        if (!str) return '';
        return str.replace(/[&<>]/g, function(m) {
            if (m === '&') return '&amp;';
            if (m === '<') return '&lt;';
            if (m === '>') return '&gt;';
            return m;
        }).replace(/[\uD800-\uDBFF][\uDC00-\uDFFF]/g, function(c) {
            return c;
        });
    }

    // 查询逻辑
    let currentStage = '正式党员';
    const stageBtns = document.querySelectorAll('.stage-btn');
    stageBtns.forEach(btn => {
        btn.addEventListener('click', () => {
            stageBtns.forEach(b => b.classList.remove('active'));
            btn.classList.add('active');
            currentStage = btn.getAttribute('data-stage');
        });
    });

    document.getElementById('searchBtn').addEventListener('click', () => {
        const name = document.getElementById('searchName').value.trim();
        if (!name) {
            alert('请输入姓名');
            return;
        }
        const member = findMemberByNameAndStage(name, currentStage);
        const resultDiv = document.getElementById('timelineResult');
        resultDiv.innerHTML = renderFullTimeline(member);
    });

    // ---------- 管理员后台 ----------
    const ADMIN_PASSWORD = 'tysy777';
    let isAdminMode = false;
    const userPanel = document.getElementById('userPanel');
    const adminPanel = document.getElementById('adminPanel');
    const toggleBtn = document.getElementById('toggleAdminBtn');
    const adminAuth = document.getElementById('adminAuth');
    const adminContent = document.getElementById('adminContent');

    function showUserMode() {
        userPanel.classList.remove('hidden');
        adminPanel.classList.add('hidden');
        isAdminMode = false;
        toggleBtn.textContent = '🔐 管理员入口';
    }

    function showAdminMode() {
        userPanel.classList.add('hidden');
        adminPanel.classList.remove('hidden');
        isAdminMode = true;
        toggleBtn.textContent = '👤 返回用户查询';
        checkAdminLogin();
    }

    function checkAdminLogin() {
        const logged = localStorage.getItem('admin_logged') === 'true';
        if (logged) {
            adminAuth.classList.add('hidden');
            adminContent.classList.remove('hidden');
            loadMemberListForAdmin();
        } else {
            adminAuth.classList.remove('hidden');
            adminContent.classList.add('hidden');
        }
    }

    function loadMemberListForAdmin() {
        const container = document.getElementById('memberList');
        if (!members.length) {
            container.innerHTML = '<div>暂无数据</div>';
            return;
        }
        let html = '';
        members.forEach(m => {
            html += `
                <div class="member-item">
                    <span><strong>${escapeHtml(m.name)}</strong> (${m.stage})</span>
                    <div>
                        <button class="edit-btn" data-id="${m.id}">编辑</button>
                        <button class="del-btn" data-id="${m.id}" data-name="${escapeHtml(m.name)}">删除</button>
                    </div>
                </div>
            `;
        });
        container.innerHTML = html;
        document.querySelectorAll('.edit-btn').forEach(btn => {
            btn.addEventListener('click', (e) => {
                const id = btn.getAttribute('data-id');
                const member = members.find(m => m.id === id);
                if (member) fillFormForEdit(member);
            });
        });
        document.querySelectorAll('.del-btn').forEach(btn => {
            btn.addEventListener('click', (e) => {
                const id = btn.getAttribute('data-id');
                const name = btn.getAttribute('data-name');
                if (confirm(`确定删除 ${name} 的记录吗？`)) {
                    members = members.filter(m => m.id !== id);
                    saveMembers();
                    loadMemberListForAdmin();
                    resetForm();
                }
            });
        });
    }

    function fillFormForEdit(member) {
        document.getElementById('editId').value = member.id;
        document.getElementById('name').value = member.name;
        document.getElementById('stage').value = member.stage;
        document.getElementById('applyDate').value = member.applyDate || '';
        document.getElementById('talkDate').value = member.talkDate || '';
        document.getElementById('activeDate').value = member.activeDate || '';
        document.getElementById('cultivator').value = member.cultivator || '';
        document.getElementById('developDate').value = member.developDate || '';
        document.getElementById('attachment2').value = member.attachment2 || '';
        document.getElementById('attachment3').value = member.attachment3 || '';
        document.getElementById('attachment6').value = member.attachment6 || '';
        document.getElementById('attachment10').value = member.attachment10 || '';
        document.getElementById('preReviewDate').value = member.preReviewDate || '';
        document.getElementById('introducer').value = member.introducer || '';
        document.getElementById('prePartyDate').value = member.prePartyDate || '';
        document.getElementById('approvePartyDate').value = member.approvePartyDate || '';
        document.getElementById('transferDate').value = member.transferDate || '';
    }

    function resetForm() {
        document.getElementById('editId').value = '';
        document.getElementById('name').value = '';
        document.getElementById('stage').value = '积极分子';
        document.getElementById('applyDate').value = '';
        document.getElementById('talkDate').value = '';
        document.getElementById('activeDate').value = '';
        document.getElementById('cultivator').value = '';
        document.getElementById('developDate').value = '';
        document.getElementById('attachment2').value = '';
        document.getElementById('attachment3').value = '';
        document.getElementById('attachment6').value = '';
        document.getElementById('attachment10').value = '';
        document.getElementById('preReviewDate').value = '';
        document.getElementById('introducer').value = '';
        document.getElementById('prePartyDate').value = '';
        document.getElementById('approvePartyDate').value = '';
        document.getElementById('transferDate').value = '';
    }

    document.getElementById('memberForm').addEventListener('submit', (e) => {
        e.preventDefault();
        const id = document.getElementById('editId').value;
        const name = document.getElementById('name').value.trim();
        const stage = document.getElementById('stage').value;
        if (!name) return alert('姓名不能为空');
        const newMember = {
            id: id || Date.now().toString(),
            name,
            stage,
            applyDate: document.getElementById('applyDate').value,
            talkDate: document.getElementById('talkDate').value,
            activeDate: document.getElementById('activeDate').value,
            cultivator: document.getElementById('cultivator').value,
            developDate: document.getElementById('developDate').value,
            attachment2: document.getElementById('attachment2').value,
            attachment3: document.getElementById('attachment3').value,
            attachment6: document.getElementById('attachment6').value,
            attachment10: document.getElementById('attachment10').value,
            preReviewDate: document.getElementById('preReviewDate').value,
            introducer: document.getElementById('introducer').value,
            prePartyDate: document.getElementById('prePartyDate').value,
            approvePartyDate: document.getElementById('approvePartyDate').value,
            transferDate: document.getElementById('transferDate').value
        };
        if (id) {
            const index = members.findIndex(m => m.id === id);
            if (index !== -1) members[index] = newMember;
        } else {
            if (members.some(m => m.name === name && m.stage === stage)) {
                alert('该姓名在此发展阶段已存在');
                return;
            }
            members.push(newMember);
        }
        saveMembers();
        alert('保存成功');
        resetForm();
        loadMemberListForAdmin();
    });

    document.getElementById('resetFormBtn').addEventListener('click', resetForm);
    document.getElementById('loginBtn').addEventListener('click', () => {
        const pwd = document.getElementById('adminPwd').value;
        if (pwd === ADMIN_PASSWORD) {
            localStorage.setItem('admin_logged', 'true');
            checkAdminLogin();
        } else {
            alert('密码错误');
        }
    });

    toggleBtn.addEventListener('click', () => {
        if (isAdminMode) {
            showUserMode();
            localStorage.removeItem('admin_logged');
        } else {
            showAdminMode();
        }
    });

    // 初始化
    loadMembersFromStorage();
    showUserMode();
</script>
</body>
</html>
