import React from "react";
import { auth, googleProvider } from "../firebase";
import { signInWithPopup, signOut } from "firebase/auth";

const Auth = ({ user, setUser }) => {
  const handleSignIn = async () => {
    try {
      const result = await signInWithPopup(auth, googleProvider);  // ✅ Fixed
      setUser(result.user);
    } catch (error) {
      console.error("Sign-in error:", error);
    }
  };

  const handleSignOut = async () => {
    try {
      await signOut(auth);
      setUser(null);
    } catch (error) {
      console.error("Sign-out error:", error);
    }
  };

  return (
    <div className="text-center">
      {user ? (
        <div>
          <img src={user.photoURL} alt="Profile" className="w-16 h-16 rounded-full mx-auto" />
          <h2 className="text-xl font-semibold">{user.displayName}</h2>
          <button className="mt-4 px-4 py-2 bg-red-500 text-white rounded" onClick={handleSignOut}>
            Sign Out
          </button>
        </div>
      ) : (
        <button className="px-4 py-2 bg-blue-500 text-white rounded" onClick={handleSignIn}>
          Sign in with Google
        </button>
      )}
    </div>
  );
};

export default Auth;
