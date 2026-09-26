<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Attendance Calculator</title>

    <style>
        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
            font-family: Arial, sans-serif;
        }

        body {
            background: linear-gradient(135deg, #667eea, #764ba2);
            min-height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
            padding: 20px;
        }

        .container {
            background: white;
            width: 100%;
            max-width: 450px;
            padding: 30px;
            border-radius: 15px;
            box-shadow: 0 10px 30px rgba(0, 0, 0, 0.2);
        }

        h1 {
            text-align: center;
            color: #333;
            margin-bottom: 10px;
        }

        .subtitle {
            text-align: center;
            color: #777;
            margin-bottom: 25px;
        }

        .input-group {
            margin-bottom: 20px;
        }

        label {
            display: block;
            font-weight: bold;
            color: #444;
            margin-bottom: 8px;
        }

        input {
            width: 100%;
            padding: 12px;
            border: 2px solid #ddd;
            border-radius: 8px;
            font-size: 16px;
            outline: none;
        }

        input:focus {
            border-color: #667eea;
        }

        button {
            width: 100%;
            padding: 13px;
            background: #667eea;
            color: white;
            border: none;
            border-radius: 8px;
            font-size: 17px;
            font-weight: bold;
            cursor: pointer;
            margin-top: 5px;
        }

        button:hover {
            background: #5568d8;
        }

        .result {
            display: none;
            margin-top: 25px;
            padding: 20px;
            border-radius: 10px;
            background: #f5f5f5;
        }

        .percentage {
            text-align: center;
            font-size: 40px;
            font-weight: bold;
            margin-bottom: 15px;
        }

        .details {
            line-height: 1.8;
            color: #444;
        }

        .details p {
            border-bottom: 1px solid #ddd;
            padding: 5px 0;
        }

        .status {
            text-align: center;
            font-weight: bold;
            font-size: 18px;
            margin-top: 15px;
            padding: 10px;
            border-radius: 8px;
        }

        .good {
            color: #087f23;
            background: #d9f7df;
        }

        .bad {
            color: #c62828;
            background: #ffdddd;
        }

        .reset {
            background: #555;
            margin-top: 10px;
        }

        .reset:hover {
            background: #333;
        }
    </style>
</head>

<body>

    <div class="container">

        <h1>Attendance Calculator</h1>
        <p class="subtitle">Calculate your attendance percentage</p>

        <div class="input-group">
            <label for="total">Total Classes</label>
            <input type="number" id="total" placeholder="Enter total classes" min="1">
        </div>

        <div class="input-group">
            <label for="attended">Classes Attended</label>
            <input type="number" id="attended" placeholder="Enter classes attended" min="0">
        </div>

        <button onclick="calculateAttendance()">
            Calculate Attendance
        </button>

        <div class="result" id="result">

            <div class="percentage" id="percentage">
                0%
            </div>

            <div class="details">
                <p>
                    Total Classes:
                    <strong id="totalResult">0</strong>
                </p>

                <p>
                    Classes Attended:
                    <strong id="attendedResult">0</strong>
                </p>

                <p>
                    Classes Absent:
                    <strong id="absentResult">0</strong>
                </p>

                <p>
                    Required Attendance:
                    <strong>75%</strong>
                </p>

                <p id="requiredClasses">
                    Required Classes: -
                </p>
            </div>

            <div class="status" id="status">
                Status
            </div>

            <button class="reset" onclick="resetCalculator()">
                Reset
            </button>

        </div>

    </div>


    <script>

        function calculateAttendance() {

            let total = parseInt(document.getElementById("total").value);
            let attended = parseInt(document.getElementById("attended").value);

            if (isNaN(total) || isNaN(attended)) {
                alert("Please enter both values.");
                return;
            }

            if (total <= 0) {
                alert("Total classes must be greater than 0.");
                return;
            }

            if (attended < 0 || attended > total) {
                alert("Classes attended cannot be greater than total classes.");
                return;
            }

            // Calculate attendance percentage
            let percentage = (attended / total) * 100;

            // Calculate absent classes
            let absent = total - attended;

            // Display basic results
            document.getElementById("percentage").innerText =
                percentage.toFixed(2) + "%";

            document.getElementById("totalResult").innerText = total;
            document.getElementById("attendedResult").innerText = attended;
            document.getElementById("absentResult").innerText = absent;

            let status = document.getElementById("status");
            let requiredClasses = document.getElementById("requiredClasses");

            /*
                Calculate how many future classes must be attended
                to reach 75%.

                (attended + x) / (total + x) >= 0.75
            */

            if (percentage >= 75) {

                status.innerText = "✓ You have 75% or more attendance";
                status.className = "status good";

                // Calculate how many classes can be missed
                let canMiss = Math.floor((attended / 0.75) - total);

                if (canMiss < 0) {
                    canMiss = 0;
                }

                requiredClasses.innerText =
                    "You can miss approximately " +
                    canMiss +
                    " more class(es) and remain at 75%.";

            } else {

                status.innerText = "✗ Attendance is below 75%";
                status.className = "status bad";

                // Required classes to reach 75%
                let x = Math.ceil((0.75 * total - attended) / 0.25);

                requiredClasses.innerText =
                    "You must attend the next " +
                    x +
                    " class(es) continuously to reach 75%.";
            }

            document.getElementById("result").style.display = "block";
        }


        function resetCalculator() {

            document.getElementById("total").value = "";
            document.getElementById("attended").value = "";

            document.getElementById("result").style.display = "none";
        }

    </script>

</body>
</html>
