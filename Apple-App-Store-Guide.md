# Step-by-Step Guide to Deploy a .NET MAUI App to the Apple App Store for an Organization

## Table of Contents
- [Prerequisites](#prerequisites)
- [Step 1: Configure Your Mac Environment](#step-1-configure-your-mac-environment)
- [Step 2: Configure Your Rider Project on Windows](#step-2-configure-your-rider-project-on-windows)
- [Step 3: Pair with Your Mac Mini](#step-3-pair-with-your-mac-mini)
- [Step 4: Build the .IPA File](#step-4-build-the-ipa-file)
- [Step 5: Prepare for App Store Submission](#step-5-prepare-for-app-store-submission)
- [Step 6: Monitor Status](#step-6-monitor-status)

---

## Prerequisites
- **Apple Developer Program Membership** (Organization account).
- **DUNS Number** (already available).  
- **Mac Mini with Xcode installed**.  
- **Windows PC with Rider** for development.  
- **.NET MAUI App Code** (complete and does not need extra permissions).  
- **iPhone or Simulator for Testing**.  

---

## Step 1: Configure Your Mac Environment
1. **Install .NET SDK for macOS:**
   - Download the latest .NET SDK for macOS: [https://dotnet.microsoft.com/download](https://dotnet.microsoft.com/download).
   - Install the SDK on your Mac Mini.

2. **Install Xcode:**
   - Open the Mac App Store and install Xcode.  
   - Accept the Xcode license:
     ```bash
     sudo xcodebuild -license
     ```
   - Install additional components when prompted.

3. **Enable Command Line Tools in Xcode:**
   - Open Xcode > Preferences > Locations.
   - Set the "Command Line Tools" dropdown to the installed version of Xcode.

4. **Install MAUI Workload:**
   - Open a terminal and run:
     ```bash
     dotnet workload install maui
     ```

5. **Install Apple Certificates and Provisioning Profiles:**
   - Log in to your Apple Developer Account on the Mac.
   - Go to **Certificates, Identifiers & Profiles** and create:
     - A distribution certificate.
     - A provisioning profile for your app’s bundle ID (e.g., `com.organization.appname`).

---

## Step 2: Configure Your Rider Project on Windows
1. **Update App Bundle Identifier:**
   - Open your `.csproj` in Rider.
   - Update the `<ApplicationId>` in `Platforms/iOS/Info.plist` to match the bundle ID you registered.

2. **Configure iOS Build Settings:**
   - Add the following configurations in the `.csproj` file:
     ```xml
     <PropertyGroup Condition="'$(TargetFramework)' == 'net7.0-ios'">
         <RuntimeIdentifier>ios-arm64</RuntimeIdentifier>
         <OutputType>Exe</OutputType>
         <ApplicationId>com.organization.appname</ApplicationId>
     </PropertyGroup>
     ```

3. **Install Dependencies Locally:**
   - In Rider’s terminal, restore dependencies:
     ```bash
     dotnet restore
     ```
   - Build the project:
     ```bash
     dotnet build -t:Run -f net7.0-ios
     ```

---

## Step 3: Pair with Your Mac Mini
1. **Enable Remote Login on Mac:**
   - Go to System Settings > Sharing > Enable **Remote Login**.
   - Note the Mac’s IP address.

2. **Pair Windows PC with Mac:**
   - Open Rider and go to **File > Preferences > Build, Execution, Deployment > iOS > Mac Connection**.
   - Add your Mac Mini’s IP address and credentials.
   - Once connected, ensure Rider detects Xcode and .NET SDK.

3. **Deploy to an iOS Device:**
   - Connect an iPhone to the Mac.
   - Build and deploy the app to the iPhone from Rider.

---

## Step 4: Build the .IPA File
1. **Switch to Release Mode:**
   - In Rider, set the configuration to **Release**.

2. **Generate the IPA:**
   - Run the following command from Rider or the terminal:
     ```bash
     dotnet publish -f net7.0-ios -c Release /p:BuildIpa=true
     ```
   - The `.ipa` file will be located in the `bin\Release\net7.0-ios\ios-arm64` directory.

---

## Step 5: Prepare for App Store Submission
1. **Install Transporter App on Mac:**
   - Download the Transporter app from the Mac App Store.

2. **Upload the IPA:**
   - Open Transporter.
   - Drag and drop the `.ipa` file.
   - Upload to App Store Connect.

3. **Fill Out App Store Connect Details:**
   - Log in to [App Store Connect](https://appstoreconnect.apple.com).
   - Create a new app with:
     - App Name.
     - Primary Language.
     - Bundle ID (select the existing one).
     - SKU (unique identifier).
   - Complete metadata:
     - Description.
     - Keywords.
     - Screenshots (take them from a simulator or iPhone).
     - App Icon (1024x1024 PNG, no alpha).

4. **Submit for Review:**
   - Navigate to **App Store Connect > My Apps > Your App > Submit for Review**.
   - Wait for Apple’s approval (usually 1–2 weeks).

---

## Step 6: Monitor Status
- Track the app's status in App Store Connect.
- Respond to any queries or required fixes from Apple.
