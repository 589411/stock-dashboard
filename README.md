# 美股成交量與 KD 監控 Dashboard

這是一個可部署到 GitHub Pages 的靜態儀表板，用來追蹤美股成交量、KD 指標，以及簡單示警條件。

## 功能

- 顯示最近 5 個交易日的收盤價、成交量、K、D、K/D 變化。
- 視覺化近 30 筆日線資料，包含收盤價、成交量、K 線與 D 線。
- 示警條件：
  - 成交量相對近 5 日均量變化超過 15%。
  - K 或 D 高於 80。
  - K 或 D 低於 20。
- 支援多股票觀察清單。
- 未上市公司可保留在設定檔註記，但不會產生日線成交量與 KD。

## 本機產生資料

```bash
pip install -r requirements.txt
python scripts/fetch_market_data.py
```

輸出檔案會寫到：

```text
docs/data/market-data.json
```

## 本機預覽

```bash
python3 -m http.server 8000 --directory docs
```

然後開啟：

```text
http://localhost:8000
```

## 增加股票

編輯 `scripts/watchlist.json`：

```json
{
  "symbols": ["MU", "GOOGL", "NVDA", "AMD", "TSM", "AAPL"],
  "privateCompanies": [
    {
      "name": "SpaceX",
      "reason": "未上市，沒有公開美股代號，無法取得成交量與 KD。"
    },
    {
      "name": "Anthropic",
      "reason": "未上市，沒有公開美股代號，無法取得成交量與 KD。"
    }
  ]
}
```

接著重新執行：

```bash
python scripts/fetch_market_data.py
```

部署到 GitHub 後，也可以手動執行 `Update market data` GitHub Action。

## GitHub Pages 部署

1. 將這個資料夾推到 GitHub repo。
2. 到 repo 的 `Settings` -> `Pages`。
3. Source 選 `Deploy from a branch`。
4. Branch 選 `main`，資料夾選 `/docs`。

GitHub Action 會在美股交易日收盤後更新 `docs/data/market-data.json`。
