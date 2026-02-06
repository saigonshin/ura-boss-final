<!DOCTYPE html>
<html lang="ja">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>【究極】もしもあなたが裏ボスだったら？</title>
    <meta property="og:title" content="【究極】もしもあなたが裏ボスだったら？">
    <meta property="og:description" content="10問の質問で、あなたの隠れた裏ボスタイプを関西弁で診断！">
    <meta property="og:image" content="boss8.png"> 
    <meta property="og:url" content="https://saigonshin.github.io/boss-diag/">
    <meta name="twitter:card" content="summary_large_image">
    <link href="https://fonts.googleapis.com/css2?family=M+PLUS+Rounded+1c:wght@700&family=Noto+Sans+JP:wght@300;700&display=swap" rel="stylesheet">
    <style>
        :root { --bg: #0f0a14; --card-bg: #1e1626; --text: #f8f8f2; --accent: #ff79c6; }
        body { margin: 0; background: var(--bg); color: var(--text); font-family: 'Noto Sans JP', sans-serif; display: flex; justify-content: center; align-items: center; min-height: 100vh; }
        #app { width: 100%; max-width: 480px; padding: 20px; box-sizing: border-box; }
        .fade-in { animation: fadeIn 0.5s ease-out; }
        @keyframes fadeIn { from { opacity: 0; } to { opacity: 1; } }
        h1 { color: var(--accent); text-align: center; font-size: 1.4rem; text-shadow: 0 0 10px rgba(255,121,198,0.5); }
        .card { background: var(--card-bg); border-radius: 20px; border: 2px solid var(--accent); overflow: hidden; text-align: center; }
        .main-img { width: 100%; height: 320px; object-fit: cover; background-color: #000; }
        .btn { display: block; width: 100%; padding: 15px; margin-top: 10px; background: #282a36; border: 2px solid #44475a; color: white; border-radius: 50px; cursor: pointer; font-size: 1rem; transition: 0.3s; border-style: solid; }
        .btn:hover { border-color: var(--accent); background: #44475a; }
        .res-desc { font-size: 0.95rem; line-height: 1.7; text-align: left; background: rgba(0,0,0,0.4); padding: 15px; border-radius: 15px; white-space: pre-wrap; margin-top: 15px; }
        .hidden { display: none; }
        .two-columns { display: grid; grid-template-columns: 1fr 1fr; gap: 8px; margin-top: 10px; }
        .two-columns .btn { margin-top: 0; font-size: 0.9rem; padding: 10px; }
    </style>
</head>
<body>
<div id="app">
    <div id="start" class="fade-in">
        <h1>【究極】もしもあなたが<br>裏ボスだったら？</h1>
        <div class="card" onclick="start()" style="cursor:pointer;">
            <img src="boss8.png" class="main-img" alt="裏ボス診断">
            <div style="padding:15px; font-weight:bold; color:var(--accent);">タップして運命を知りや！</div>
        </div>
    </div>
    <div id="quiz" class="hidden fade-in">
        <div class="card" style="padding:20px; border-width:2px;">
            <p id="q-text" style="font-weight:bold; font-size:1.1rem; margin-bottom:20px;"></p>
            <div id="options"></div>
        </div>
    </div>
    <div id="result" class="hidden fade-in">
        <div id="res-card" class="card" style="border-width:4px;">
            <img id="res-img" class="main-img" src="" alt="診断結果">
            <div style="padding:20px;">
                <div id="res-type" style="font-size:1.4rem; font-weight:bold; margin-bottom:10px;"></div>
                <div id="res-desc" class="res-desc"></div>
            </div>
        </div>
        <button class="btn" onclick="location.reload()" style="border-color:var(--accent); margin-top:20px;">もう一回遊ぶんか？</button>
        <p style="text-align:center; font-size:0.75rem; opacity:0.6; margin-top:15px;">Produced by YUMIKO IDUKI</p>
    </div>
</div>
<script>
    const questions = [
        { q: "Q1. まずは血液型を教えてーな？", opts: [{t: "A型", s: "A"}, {t: "B型", s: "B"}, {t: "O型", s: "C"}, {t: "AB型", s: "D"}] },
        { q: "Q2. 自分の星座を選んでや！", opts: [
            {t: "おひつじ座", s: "B"}, {t: "おうし座", s: "A"}, {t: "ふたご座", s: "D"}, {t: "かに座", s: "C"},
            {t: "しし座", s: "B"}, {t: "おとめ座", s: "A"}, {t: "てんびん座", s: "D"}, {t: "さそり座", s: "C"},
            {t: "いて座", s: "B"}, {t: "やぎ座", s: "A"}, {t: "みずがめ座", s: "D"}, {t: "うお座", s: "C"}
        ], isGrid: true },
        { q: "Q3. 喧嘩が始まったらどないする？", opts: [{t: "離れてじーっと見てる", s: "A"}, {t: "面白そうやん！って混ざる", s: "B"}, {t: "まぁまぁ、落ち着きやって止める", s: "C"}, {t: "どっち勝たせるか裏で操る", s: "D"}] },
        { q: "Q4. 隠し事してる時の気分は？", opts: [{t: "特になんも思わん", s: "A"}, {t: "バレた時の顔が見たいわぁ", s: "B"}, {t: "ちょっと心苦しいなぁ", s: "C"}, {t: "勝つための戦略やろ", s: "D"}] },
        { q: "Q5. 魔法ひとつ使えるなら？", opts: [{t: "世界の記憶を覗き見たい", s: "A"}, {t: "常識をメチャクチャにしたい", s: "B"}, {t: "みんなに好かれまくりたい", s: "C"}, {t: "未来を自分の思い通りにしたい", s: "D"}] },
        { q: "Q6. 弱点、他人に見せる？", opts: [{t: "絶対見せへんわ", s: "A"}, {t: "笑いのネタにする", s: "B"}, {t: "仲良くなる武器にする", s: "C"}, {t: "弱点に見せかけた罠を張る", s: "D"}] },
        { q: "Q7. 計画通りにいかなかったら？", opts: [{t: "黙って修正する", s: "A"}, {t: "ハプニングを楽しむ", s: "B"}, {t: "周りに助けてもらう", s: "C"}, {t: "その状況さえも利用する", s: "D"}] },
        { q: "Q8. 休みの日は何してることが多い？", opts: [{t: "一人で読書やゲーム", s: "A"}, {t: "刺激を求めて外出", s: "B"}, {t: "友達とお喋り", s: "C"}, {t: "人間観察や情報収集", s: "D"}] },
        { q: "Q9. 正直、世界をどないしたい？", opts: [{t: "静かに暮らせればええ", s: "A"}, {t: "もっとハチャメチャにしたい", s: "B"}, {t: "愛で満たしたい", s: "C"}, {t: "自分の理想通りに作り替えたい", s: "D"}] },
        { q: "Q10. 勝った時の決め台詞は？", opts: [{t: "「想定内やな」", s: "A"}, {t: "「あーおもろかった！」", s: "B"}, {t: "「みんな、ウチの下に来る？」", s: "C"}, {t: "「はい、詰みやね」", s: "D"}] }
    ];

    const resData = {
        A: { t: "インテリ眼鏡の冷徹執行官", d: "自分、頭良すぎて逆に怖いわ！お茶すすりながら「全部、計算通りやで」って涼しい顔で言うてるタイプやろ。感情に流されへんから、敵に回すと一番理詰めでボコボコにされる、最強のインテリやな。", c: "#4cc9f0", i: "boss1.png" },
        B: { t: "爆破大好き！ガトリング少女", d: "自分、テンション高すぎやろ！「ルール？そんなん壊すためにあるんやろ？」って笑いながら、デカい銃をぶっ放すタイプやな。自分が楽しければ世界がどないなってもええっていう、無茶苦茶やけど皆に好かれる爆弾娘やで！", c: "#f72585", i: "boss2.png" },
        C: { t: "毒薬と微笑みのマッドサイエンティスト", d: "自分、顔は笑ってるけど目が全然笑ってへんで！優しく話しかけながら、裏でこっそり怪しい薬を作って「どんな反応するか楽しみやなぁ」とか言うてそうやわ。本気で怒らせたら一番えげつない、変態的な天才やな！", c: "#ffbe0b", i: "boss3.png" },
        D: { t: "影を操る虚無の支配者", d: "自分、ミステリアスすぎて存在が「闇」そのものやん！言葉少なめに「…消えろ」って一言で相手を沈める正統派。深い孤独を抱えてる感じが、逆にかっこよすぎてファンが仰山つくタイプやで。", c: "#7209b7", i: "boss4.png" },
        E: { t: "ぬいぐるみと残酷なプリンセス", d: "見た目は可愛らしいお嬢様やのに、中身は驚くほど残酷やな！「あの子、邪魔やから消して？」ってぬいぐるみに命令してそうやわ。その可愛さと恐ろしさのギャップに、一生下僕にされる奴が後を絶たへんで！", c: "#ff0054", i: "boss5.png" },
        F: { t: "墜ちた聖職者の光と闇", d: "自分、一番タチ悪いタイプやな！正義の顔して「救ってあげます」って言いながら、絶望に突き落とすんやろ？背中の羽が白か黒か分からんくらいオーラが複雑や。自分に助けられたら最後、そこは地獄の入り口やわ。", c: "#480ca8", i: "boss6.png" },
        Mix: { t: "全てを嘲笑う伝説の神", d: "自分、もう次元が違うわ！男とか女とか関係ない、存在そのものが「災害」レベル。全ての裏ボスが束になっても勝てへん真のリーダーや。自分が出てきたら物語は強制終了。自分、最強すぎて人生退屈してへんか？", c: "#ff79c6", i: "boss7.png" }
    };

    let cur = 0, scores = {A:0, B:0, C:0, D:0};
    function start() { document.getElementById('start').classList.add('hidden'); document.getElementById('quiz').classList.remove('hidden'); show(); }
    function show() {
        const q = questions[cur];
        document.getElementById('q-text').innerText = q.q;
        const optDiv = document.getElementById('options');
        optDiv.innerHTML = '';
        optDiv.className = q.isGrid ? 'two-columns' : '';
        q.opts.forEach(o => {
            const b = document.createElement('button');
            b.className = 'btn';
            b.innerText = o.t;
            b.onclick = () => { scores[o.s]++; next(); };
            optDiv.appendChild(b);
        });
    }
    function next() { cur++; if(cur < questions.length) show(); else result(); }
    function result() {
        document.getElementById('quiz').classList.add('hidden');
        document.getElementById('result').classList.remove('hidden');
        let win = Object.keys(scores).reduce((a, b) => scores[a] > scores[b] ? a : b);
        const max = scores[win];
        const isMix = Object.values(scores).filter(s => s === max).length >= 2;
        let res;
        if(isMix) res = resData.Mix;
        else {
            if(win === 'A' && scores.A >= 5 && Math.random() > 0.5) res = resData.F;
            else if(win === 'B' && scores.B >= 5 && Math.random() > 0.5) res = resData.E;
            else res = resData[win];
        }
        document.getElementById('res-type').innerText = "【" + res.t + "】";
        document.getElementById('res-desc').innerText = res.d;
        document.getElementById('res-img').src = res.i;
        document.getElementById('res-card').style.borderColor = res.c;
    }
</script>
</body>
</html>
