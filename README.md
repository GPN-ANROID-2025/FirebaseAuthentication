# Firebase Authentication in Java (Android) with Email & Password

- 
### Steps Using Android Studio Assistant

1. Open **Android Studio**.
2. Go to **Tools** > **Firebase**.
3. Expand **Authentication** and select **Email & Password Authentication**.
4. Click **Connect to Firebase** and follow the setup instructions.
5. Click **Add Firebase Authentication to your app** to add dependencies automatically.
6. Implement the authentication methods in your app as described above.


## Step 1: Set Up Firebase

1. Go to [Firebase Console](https://console.firebase.google.com/)
2. Create a new Firebase project.
3. Add an Android app to your Firebase project and register it.
4. Download the `google-services.json` file and place it in `app/` of your Android project.
5. Enable **Email/Password Authentication**:
   - Navigate to **Authentication** > **Sign-in method** in Firebase Console.
   - Enable **Email/Password**.

## Step 2: Add Firebase Dependencies

In `app/build.gradle`, add the Firebase authentication dependency:

```gradle
implementation 'com.google.firebase:firebase-auth:21.0.1'
```

Ensure `google-services` plugin is applied in `build.gradle (Project)`:

```gradle
classpath 'com.google.gms:google-services:4.3.10'
```

Sync the project.

## Step 3: Initialize Firebase Authentication

In your `MainActivity.java`:

```java
import com.google.firebase.auth.FirebaseAuth;
import com.google.firebase.auth.FirebaseUser;

public class MainActivity extends AppCompatActivity {
    private FirebaseAuth mAuth;
    
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        
        mAuth = FirebaseAuth.getInstance();
    }
}
```

## Step 4: Implement User Registration

Create a method for user sign-up:

```java
private void registerUser(String email, String password) {
    mAuth.createUserWithEmailAndPassword(email, password)
        .addOnCompleteListener(this, task -> {
            if (task.isSuccessful()) {
                FirebaseUser user = mAuth.getCurrentUser();
                Toast.makeText(MainActivity.this, "Registration successful!", Toast.LENGTH_SHORT).show();
            } else {
                Toast.makeText(MainActivity.this, "Registration failed: " + task.getException().getMessage(), Toast.LENGTH_SHORT).show();
            }
        });
}
```

## Step 5: Implement User Login

Create a method for user sign-in:

```java
private void loginUser(String email, String password) {
    mAuth.signInWithEmailAndPassword(email, password)
        .addOnCompleteListener(this, task -> {
            if (task.isSuccessful()) {
                FirebaseUser user = mAuth.getCurrentUser();
                Toast.makeText(MainActivity.this, "Login successful!", Toast.LENGTH_SHORT).show();
            } else {
                Toast.makeText(MainActivity.this, "Login failed: " + task.getException().getMessage(), Toast.LENGTH_SHORT).show();
            }
        });
}
```

## Step 6: Check User Login Status

```java
@Override
public void onStart() {
    super.onStart();
    FirebaseUser currentUser = mAuth.getCurrentUser();
    if (currentUser != null) {
        Toast.makeText(this, "User already logged in", Toast.LENGTH_SHORT).show();
    }
}
```

## Step 7: Logout User

```java
private void logoutUser() {
    mAuth.signOut();
    Toast.makeText(MainActivity.this, "Logged out successfully!", Toast.LENGTH_SHORT).show();
}
```

## Step 8: Implement Password Reset

Create a method to send a password reset email:

```java
private void sendPasswordResetEmail(String email) {
    mAuth.sendPasswordResetEmail(email)
        .addOnCompleteListener(task -> {
            if (task.isSuccessful()) {
                Toast.makeText(MainActivity.this, "Password reset email sent!", Toast.LENGTH_SHORT).show();
            } else {
                Toast.makeText(MainActivity.this, "Failed to send reset email: " + task.getException().getMessage(), Toast.LENGTH_SHORT).show();
            }
        });
}
```


