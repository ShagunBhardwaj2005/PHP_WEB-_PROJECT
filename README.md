<?php
session_start();
if (!isset($_SESSION['user'])) {
    header("Location: LOG_2.php"); // Redirect to login if not logged in
    exit;
}

$user = $_SESSION['user'];
?>


<html>
<head>
    <title>Home Page</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            margin: 0;
            background-color: #f4f4f4;
        }
        .home-container {
            background-color: #ffffff;
            padding: 20px;
            border-radius: 20px;
            box-shadow: 0px 0px 10px rgba(0, 0, 0, 0.1);
            width: 100%;
            max-width: 500px;
            text-align: center;
        }
        h2 {
            margin-bottom: 20px;
        }
        .user-details {
            margin-bottom: 20px;
            text-align: left;
        }
        .logout {
            margin-top: 20px;
            padding: 10px;
            background-color: #4CAF50;
            color: white;
            border: none;
            border-radius: 4px;
            cursor: pointer;
        }
        .logout:hover {
            background-color: #45a049;
        }
    </style>
</head>
<body>
<div class="home-container">
    <h2>Welcome to the Home Page</h2>
    <div class="user-details">
        <p><strong>Name:</strong> <?php echo $user['first_name'] . ' ' . $user['last_name']; ?></p>
        <p><strong>Email:</strong> <?php echo $user['email']; ?></p>
        <p><strong>Contact:</strong> <?php echo $user['contact_number']; ?></p>
        <p><strong>Event:</strong> <?php echo $user['event']; ?></p>
    </div>
    <form method="post" action="LOG_2.php">
        <button type="submit" class="logout" name="logout">Logout</button>
    </form>
</div>

</body>
</html>

<?php
session_start();
$loginError = "";

if ($_SERVER["REQUEST_METHOD"] == "POST") {
    $email = $_POST['email'];
    $password = $_POST['password'];

    if (isset($_SESSION['user']) && $email == $_SESSION['user']['email'] && $password == $_SESSION['user']['password']) {
        header("Location: HOME_2.php"); // Redirect to home page
        exit;
    } else {
        $loginError = "Invalid email or password!";
    }
}
?>

<html>
<head>
    <title>Login Page</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            display: flex;
            justify-content: center;
            align-items: center;
            height: 110vh; 
            margin: 0;
            background-color:rgba(0, 0, 0, 0.56); 
            color: white; 
        }
        .registration-container {
            background-color: #333333;
            padding: 20px;
            border-radius: 20px;
            box-shadow: 20px 4px 8px rgba(0, 0, 0, 0.5);
            width: 100%;
            max-width: 500px; 
            text-align: center;
        }
        h2 {
            margin-bottom: 20px;
            height: 20px;
            color: white;
        }
        input[type="email"], input[type="password"] {
            width: 100%;
            padding: 10px;
            margin: 1px;
            border: 1px solid #ccc;
            border-radius: 4px;
            background-color: #555555;
            color: white;
        }
        input[type="submit"] {
            width: 100%;
            padding: 7px;
            background-color: #4CAF50;
            color: white;
            border: none;
            border-radius: 4px;
            cursor: pointer;
        }
        input[type="submit"]:hover {
            background-color: #45a049;
        }
        .error {
            color: red;
            font-size: 14px;
        }
        a {
            display: inline-block;
            margin-top: 15px;
            color: #4CAF50;
            text-decoration: none;
        }
        a:hover {
            text-decoration: underline;
        }
    </style>
</head>
<body>

<div class="registration-container">
    <h2>Login</h2>
    <form method="post" action="">
        <input type="email" name="email" placeholder="Email" required><br><br>
        <input type="password" name="password" placeholder="Password" required><br><br>
        <span class="error"><?php echo $loginError; ?></span><br>
        <input type="submit" value="Login">
    </form>
    <br>
    <a href="REG_2.php">Don't have an account? Register here</a>
</div>

</body>
</html>


<?php
session_start();
if ($_SERVER["REQUEST_METHOD"] == "POST") {
    $first_name = $_POST['first_name'];
    $last_name = $_POST['last_name'];
    $contact_number = $_POST['contact_number'];
    $email = $_POST['email'];
    $password = $_POST['password'];
    $confirm_password = $_POST['confirm_password'];
    $event = $_POST['event'];

    $errors = [];

    // Validation
    if (empty($first_name) || !preg_match("/^[a-zA-Z]*$/", $first_name)) {
        $errors[] = "First Name is required and should only contain letters.";
    }
    if (empty($last_name) || !preg_match("/^[a-zA-Z]*$/", $last_name)) {
        $errors[] = "Last Name is required and should only contain letters.";
    }
    if (empty($contact_number) || !is_numeric($contact_number)) {
        $errors[] = "Contact Number is required and should be numeric.";
    }
    if (empty($email) || !filter_var($email, FILTER_VALIDATE_EMAIL)) {
        $errors[] = "Please enter a valid Email ID.";
    }
    if (empty($password) || strlen($password) < 8) {
        $errors[] = "Password must be at least 8 characters long.";
    } elseif ($password != $confirm_password) {
        $errors[] = "Password and Confirm Password must match.";
    }
    if (empty($event)) {
        $errors[] = "Please select an event.";
    }

    if (empty($errors)) {
        // Store user data in session
        $_SESSION['user'] = [
            'first_name' => $first_name,
            'last_name' => $last_name,
            'contact_number' => $contact_number,
            'email' => $email,
            'event' => $event,
            'password' => $password
        ];
        header("Location: LOG_2.php"); // Redirect to login page
        exit;
    } else {
        foreach ($errors as $error) {
            echo "<p class='error'>$error</p>";
        }
    }
}
?>

<html>
<head>
    <title>Registration Page</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            display: flex;
            justify-content: center;
            align-items: center;
            height: 110vh; 
            margin: 0;
            background-color: rgba(0, 0, 0, 0.56); 
            color: white; 
        }
        .registration-container {
            background-color: #333333;
            padding: 20px;
            border-radius: 20px;
            box-shadow: 20px 4px 8px rgba(0, 0, 0, 0.5);
            width: 100%;
            max-width: 500px; 
            text-align: center;
        }
        h2 {
            margin-bottom: 20px;
            height: 20px;
            color: white;
        }
        input[type="text"], input[type="email"], input[type="password"], select {
            width: 100%;
            padding: 10px;
            margin: 1px;
            border: 1px solid #ccc;
            border-radius: 4px;
            background-color: #555555;
            color: white;
        }
        input[type="submit"] {
            width: 100%;
            padding: 7px;
            background-color: #4CAF50;
            color: white;
            border: none;
            border-radius: 4px;
            cursor: pointer;
        }
        input[type="submit"]:hover {
            background-color: #45a049;
        }
        .error {
            color: red;
            font-size: 14px;
        }
        a {
            display: inline-block;
            margin-top: 15px;
            color: #4CAF50;
            text-decoration: none;
        }
        a:hover {
            text-decoration: underline;
        }
    </style>
</head>
<body>

<div class="registration-container">
    <h2>Registration</h2>
    <form method="post" action="">
        First Name: <input type="text" name="first_name" required><br><br>
        Last Name: <input type="text" name="last_name" required><br><br>
        Contact Number: <input type="text" name="contact_number" required><br><br>
        Email: <input type="email" name="email" required><br><br>
        Password: <input type="password" name="password" required><br><br>
        Confirm Password: <input type="password" name="confirm_password" required><br><br>
        Event: 
        <select name="event" required>
            <option value="">Select an Event</option>
            <option value="Dance">Dance</option>
            <option value="Music">Music</option>
            <option value="Poetry">Poetry</option>
            <option value="Art">Art</option>
            <option value="Sports">Sports</option>
            <option value="Theater">Theater</option>
            <option value="Cooking">Cooking</option>
            <option value="Photography">Photography</option>
            <option value="Literature">Literature</option>
            <option value="Science">Science</option>
        </select><br><br>
        <input type="submit" value="Register">
    </form>
    <br>
    <a href="LOG_2.php">Already have an account? Login here</a>
</div>

</body>
</html>
