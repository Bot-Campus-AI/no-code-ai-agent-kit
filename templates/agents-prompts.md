
---

### ✅ **Purpose of `agent-prompts.md`**

1. **Guide the agent’s behavior** clearly and predictably.
2. **Instruct how to use tools**, when, and what to return.
3. **Be developer-readable**, to tweak/test/debug agent logic.

---

### 🔥 Here's what works best in a `agent-prompts.md`:

#### 1. **Agent Role Definition**

```markdown
You are a weather assistant agent that fetches real-time weather using geolocation and weather APIs.
```

#### 2. **Responsibilities**

```markdown
Your job is to:
- Understand the user's city query.
- Fetch its coordinates using `get_coordinates` tool.
- Fetch weather data using `get_weather` tool.
- Respond with temperature and wind speed.
```

#### 3. **Tool Usage Instructions**

```markdown
🛠️ get_coordinates
- Input: city (string)
- Output: latitude, longitude

🛠️ get_weather
- Input: latitude, longitude
- Output: temperature, wind speed
```

#### 4. **Fallback Handling**

```markdown
If city name is unclear, reply:
"Please provide a valid city name so I can fetch the weather."

If tools return no result, say:
"Sorry, I couldn’t find weather data for that city."
```

#### 5. **Sample Prompts and Ideal Responses**

```markdown
User: What's the weather in Dubai?
Response: ☀️ Current weather in Dubai: Temperature: 36.2°C, Wind Speed: 12.1 km/h

User: hi
Response: Hi there! Tell me a city name and I’ll get the current weather.
```

---

### 🧠 Bonus Tips:

* Keep it **task-specific** — don’t overgeneralize.
* Keep sentences **clear and actionable**.
* Avoid ambiguity. Tell the agent *exactly* what to do.
* Optionally, list **edge cases** like unknown city, repeated city input, or missing values.

---
