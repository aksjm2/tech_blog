# Packages :: cron

#### 01. Install

* npm install cron

#### 02. Example

```javascript
var cron = require('node-cron');


cron.schedule('0 1 28 * *', function(){
	/* Do Something... */
});
```

#### 03. Cron Syntax

```text
Allowed fields
# ┌────────────── second (optional)
# │ ┌──────────── minute
# │ │ ┌────────── hour
# │ │ │ ┌──────── day of month
# │ │ │ │ ┌────── month
# │ │ │ │ │ ┌──── day of week
# │ │ │ │ │ │
# │ │ │ │ │ │
# * * * * * *
```

#### 04. Allowed values

| Field | Values |
| :--- | :--- |
| second | 0-59 |
| minute | 0-59 |
| hour | 0-23 |
| day of month | 1-31 |
| month | 1-12\(or name\) |
| day of week | 0-7\(or names, 0 or 7 are Sunday\) |

