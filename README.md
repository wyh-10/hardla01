<!DOCTYPE html>
<html lang="zh">
<head>
  <meta charset="UTF-8">
  <title>文本对比工具</title>
  <style>
    * { box-sizing: border-box; margin: 0; padding: 0; }
    body { font-family: monospace; padding: 20px; background: #f5f5f5; }
    h2 { margin-bottom: 16px; font-size: 18px; color: #333; }

    .container {
      display: flex;
      gap: 16px;
      margin-bottom: 16px;
    }

    .input-box {
      flex: 1;
      display: flex;
      flex-direction: column;
      gap: 8px;
    }

    label { font-weight: bold; color: #555; font-size: 14px; }

    textarea {
      width: 100%;
      height: 200px;
      padding: 10px;
      border: 1px solid #ccc;
      border-radius: 6px;
      font-family: monospace;
      font-size: 14px;
      resize: vertical;
    }

    button {
      padding: 10px 28px;
      background: #4a90e2;
      color: #fff;
      border: none;
      border-radius: 6px;
      font-size: 15px;
      cursor: pointer;
      margin-bottom: 20px;
    }
    button:hover { background: #357abd; }

    .result-section { margin-bottom: 20px; }

    .result-section h3 {
      font-size: 15px;
      margin-bottom: 8px;
      color: #333;
    }

    .tag-list {
      display: flex;
      flex-wrap: wrap;
      gap: 8px;
    }

    .tag {
      padding: 4px 12px;
      border-radius: 4px;
      font-size: 14px;
      font-family: monospace;
    }

    .tag-only-a { background: #fde8e8; color: #c0392b; border: 1px solid #f5c6c6; }
    .tag-only-b { background: #e8f4fd; color: #2471a3; border: 1px solid #c6e0f5; }
    .tag-common { background: #eafaf1; color: #1e8449; border: 1px solid #c6efce; }

    .empty { color: #999; font-size: 13px; }
  </style>
</head>
<body>

<h2>文本对比工具</h2>

<div class="container">
  <div class="input-box">
    <label>输入 A</label>
    <textarea id="inputA" placeholder="每行一条数据...">DHU665G-2943
DHU665G-2942
DHU665G-2941
DHU665G-2940
DHU665G-2939
DHU665G-3157
DHU665G-3155
DHU665G-3154</textarea>
  </div>
  <div class="input-box">
    <label>输入 B</label>
    <textarea id="inputB" placeholder="每行一条数据...">DHU665G-3157
DHU665G-3155
DHU665G-3154</textarea>
  </div>
</div>

<button onclick="compare()">开始对比</button>

<div id="result"></div>

<script>
  function compare() {
    // 按行分割，去掉空行和首尾空格
    const parseLines = str => str.split('\n').map(l => l.trim()).filter(l => l !== '');

    const setA = new Set(parseLines(document.getElementById('inputA').value));
    const setB = new Set(parseLines(document.getElementById('inputB').value));

    // A 有 B 没有
    const onlyInA = [...setA].filter(v => !setB.has(v));
    // B 有 A 没有
    const onlyInB = [...setB].filter(v => !setA.has(v));
    // 两者都有
    const common  = [...setA].filter(v =>  setB.has(v));

    const renderTags = (arr, cls) => arr.length
      ? arr.map(v => `<span class="tag ${cls}">${v}</span>`).join('')
      : '<span class="empty">（无）</span>';

    document.getElementById('result').innerHTML = `
      <div class="result-section">
        <h3>🔴 仅在 A 中（A 有，B 没有）— ${onlyInA.length} 条</h3>
        <div class="tag-list">${renderTags(onlyInA, 'tag-only-a')}</div>
      </div>
      <div class="result-section">
        <h3>🔵 仅在 B 中（B 有，A 没有）— ${onlyInB.length} 条</h3>
        <div class="tag-list">${renderTags(onlyInB, 'tag-only-b')}</div>
      </div>
      <div class="result-section">
        <h3>🟢 两者相同 — ${common.length} 条</h3>
        <div class="tag-list">${renderTags(common, 'tag-common')}</div>
      </div>
    `;
  }
</script>

</body>
</html>
