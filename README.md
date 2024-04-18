# Comp1004 code project
********************************************************************************************************************************************************
this code here is the project and website I been designing and working on for a very long time, it is a fitness tracker that enables users to track their own exercises throughout their duration and calories burned during their time working out, I have even implemented a graph to make it easier for a lot of users who prefer to see their progress visually. 

I am also keen to tell you about the BMI calculator that has been installed, I have made it so that all ages and genders have been included as I believe every user deserves a chance to be fairly measured and have an accurate scaling to their BMIs, this is all to promote a healthier lifestyle for the users who use my project.

I have also made it so that the user has no problem navigating my website with a simplistic UI as I account for all age groups and wish to make this as seamless for anyone who comes across it.

********************************************************************************************************************************************************







<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Fitness Tracker</title>
  <!-- Bootstrap CSS -->
  <link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.5.2/css/bootstrap.min.css">
  <!-- Chart.js -->
  <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
  <style>
    body {
      font-family: 'Helvetica', sans-serif;
      background-color: #f8f9fa;
      margin: 0;
      padding: 0;
      display: flex;
      justify-content: center;
      align-items: center;
      min-height: 100vh;
    }

    .container {
      max-width: 600px;
      width: 100%;
      padding: 20px;
      border-radius: 10px;
      box-shadow: 0 0 20px rgba(0, 0, 0, 0.1);
      background-color: #ffffff;
    }

    .card-header {
      background-color: #28a745;
      color: #ffffff;
      font-size: 28px;
      text-align: center;
      padding: 15px 0;
      border-radius: 10px 10px 0 0;
    }

    .card-body {
      padding: 20px;
    }

    .form-group {
      margin-bottom: 20px;
    }

    .form-control {
      border-radius: 5px;
    }

    button {
      background-color: #28a745;
      color: #ffffff;
      border: none;
      border-radius: 5px;
      padding: 10px 20px;
      cursor: pointer;
    }

    button:hover {
      background-color: #218838;
    }

    canvas {
      max-width: 100%;
      height: auto;
    }

    .mt-4 {
      margin-top: 20px;
    }

    h5 {
      color: #28a745;
    }

    p {
      margin: 0;
    }

    .alert {
      padding: 15px;
      margin-bottom: 20px;
      border-radius: 5px;
    }

    .alert-danger {
      background-color: #dc3545;
      color: #ffffff;
    }

    .alert-success {
      background-color: #28a745;
      color: #ffffff;
    }
  </style>
</head>
<body>
  <div class="container">
    <div class="card">
      <div class="card-header">
        Fitness Tracker
      </div>
      <div class="card-body">
        <form id="trackerForm">
          <!-- Exercise Form Fields -->
          <div class="form-group">
            <label for="exercise">Exercise:</label>
            <input type="text" class="form-control" id="exercise" placeholder="Enter exercise" required>
          </div>

          <!-- Duration Form Fields -->
          <div class="form-group">
            <label for="duration">Duration (minutes):</label>
            <input type="number" class="form-control" id="duration" placeholder="Enter duration" required>
          </div>

          <!-- Track Exercise Button -->
          <button type="button" class="btn btn-success btn-block" onclick="trackExercise()">Track Exercise</button>
        </form>

        <!-- Exercise History Chart -->
        <div class="mt-4">
          <h5>Exercise History</h5>
          <canvas id="exerciseChart"></canvas>
        </div>

        <!-- Calories Burned Section -->
        <div class="mt-4">
          <h5>Calories Burned</h5>
          <p id="calories"></p>
        </div>

        <!-- BMI Calculator Section -->
        <div class="mt-4">
          <h5>BMI Calculator</h5>
          <!-- Weight Form Field -->
          <div class="form-group">
            <label for="weight">Weight (kg):</label>
            <input type="number" class="form-control" id="weight" placeholder="Enter weight" required>
          </div>

          <!-- Height Form Field -->
          <div class="form-group">
            <label for="height">Height (cm):</label>
            <input type="number" class="form-control" id="height" placeholder="Enter height" required>
          </div>

          <!-- Age Form Field -->
          <div class="form-group">
            <label for="age">Age:</label>
            <input type="number" class="form-control" id="age" placeholder="Enter age" required>
          </div>

          <!-- Gender Form Field -->
          <div class="form-group">
            <label for="gender">Gender:</label>
            <select class="form-control" id="gender" required>
              <option value="male">Male</option>
              <option value="female">Female</option>
            </select>
          </div>

          <!-- Calculate BMI Button -->
          <button type="button" class="btn btn-primary btn-block" onclick="calculateBMI()">Calculate BMI</button>
          <p id="bmiResult"></p>
          <p id="advice"></p>
        </div>
      </div>
    </div>
  </div>

  <script>
    const exerciseData = {
      labels: [],
      datasets: [{
        label: 'Duration (minutes)',
        backgroundColor: 'rgba(75, 192, 192, 0.2)',
        borderColor: 'rgba(75, 192, 192, 1)',
        borderWidth: 1,
        data: [],
      }],
    };

    const ctx = document.getElementById('exerciseChart').getContext('2d');
    const myLineChart = new Chart(ctx, {
      type: 'line',
      data: exerciseData,
    });

    function trackExercise() {
      const exercise = document.getElementById('exercise').value;
      const duration = document.getElementById('duration').value;

      if (exercise && duration) {
        exerciseData.labels.push(exercise);
        exerciseData.datasets[0].data.push(parseInt(duration, 10));
        myLineChart.update();

        const caloriesBurned = calculateCaloriesBurned(duration, exercise);
        document.getElementById('calories').innerText = `Calories Burned: ${caloriesBurned.toFixed(2)}`;

        document.getElementById('trackerForm').reset();
      } else {
        alert('Please enter all required information.');
      }
    }

    function calculateCaloriesBurned(duration, exerciseType) {
      const lowerCaseExerciseType = exerciseType.toLowerCase();

      const calorieBurnRates = {
        running: 10,
         cycling: 8,
         swimming: 12,
         jumping_rope: 15,
         weightlifting: 5,
         yoga: 3,
         dancing: 7,
         basketball: 9,
         hiking: 6,
         kickboxing: 11,
         rowing: 10,
         skiing: 13,
         stair_climbing: 8,
         elliptical: 9,
         pushups: 7,
         situps: 6,
         gardening: 5,
         martial_arts: 10,
         pilates: 4,
         skateboarding: 5,
         surfing: 7,
         volleyball: 6,
         badminton: 5,
         table_tennis: 4,
         rock_climbing: 12,
         frisbee: 6,
         ice_skating: 10,
         snowboarding: 11,
         horseback_riding: 4,
         canoing: 7,
         fishing: 3,
         golf: 4,
         kayaking: 6,
         water_aerobics: 6,
         zumba: 8,
         power_walking: 5,
         trampoline_jumping: 9,
         crossfit: 12,
         inline_skating: 10,
         baseball: 7,
         softball: 6,
         bodybuilding: 8,
         surfing: 7,
         windsurfing: 9,
         fencing: 8,
         juggling: 5,
         jump_squats: 10,
         paddleboarding: 6,
         snorkeling: 7,
         hot_yoga: 5,
         mountain_biking: 10,
         parkour: 11,
         kite_flying: 4,
         rollerblading: 9,
         water_polo: 10,
         horse_riding: 5,
         trapeze: 7,
         ultimate_frisbee: 8,
         snowshoeing: 7,
         calisthenics: 8,
         circuit_training: 9,
         skate_skiing: 12,
         paintball: 6,
      };

      const caloriesPerMinute = calorieBurnRates[lowerCaseExerciseType] || 5;
      const burnedCalories = parseInt(duration, 10) * caloriesPerMinute;

      return burnedCalories;
    }

    function calculateBMI() {
      const weight = document.getElementById('weight').value;
      const height = document.getElementById('height').value;
      const age = document.getElementById('age').value;
      const gender = document.getElementById('gender').value;

      if (weight && height && age && gender) {
        const bmi = (weight / Math.pow(height / 100, 2)).toFixed(2);
        document.getElementById('bmiResult').innerText = `Your BMI: ${bmi} (${getBMICategory(bmi)})`;
        displayBMIAdvice(bmi, age, gender);
      } else {
        alert('Please enter all required information.');
      }
    }

    function getBMICategory(bmi) {
      if (bmi < 18.5) return 'Underweight';
      if (bmi < 24.9) return 'Normal weight';
      if (bmi < 29.9) return 'Overweight';
      return 'Obese';
    }

    function displayBMIAdvice(bmi, age, gender) {
      const adviceDiv = document.getElementById('advice');
      adviceDiv.innerHTML = '';

      if (bmi < 18.5) {
        adviceDiv.innerHTML = 'You are underweight. Consider consulting with a healthcare professional for advice.';
      } else if (bmi < 24.9) {
        adviceDiv.innerHTML = 'You have a normal weight. Keep up the good work!';
      } else if (bmi < 29.9) {
        adviceDiv.innerHTML = 'You are overweight. Consider incorporating regular exercise and a balanced diet.';
      } else {
        adviceDiv.innerHTML = 'You are in the obese category. It is advisable to consult with a healthcare professional for guidance.';
      }

      if (age < 18) {
        adviceDiv.innerHTML += '<br>Consider involving a parent or healthcare professional in your fitness journey.';
      }

      if (gender === 'female') {
        adviceDiv.innerHTML += '<br>Women may have different nutritional needs. Ensure a balanced diet rich in essential nutrients.';
      }
    }
  </script>
</body>
</html>
