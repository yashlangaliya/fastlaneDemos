default_platform(:ios)

platform :ios do 
    xcode_select "/Applications/Xcode 10.1.app" #select xcode version for building app
  	build = get_info_plist_value(path: "./XXXpathXXX/Info.plist", key: "CFBundleVersion") #select to get app's bundle id
  	
    #folder ids to upload generated build to google drive
    folderIds = {
		"QA" => "1gUzg0vk09Y_dFDzOu3NIf0Lv2Tiv45PD",
		"PreProd" => "13w-hjjKZ5ZZPeoHoSfO0IZdzVwrBTsmH",
		"Production" => "1PryEJXMgTmeMvicmlI6CmYrmAqG2nMU_",
		"UAT" => "17-xuXaTGrokF6NjsW6E0EXgqBs2AQZ6i"
	}

	paths = {
		"QA" => "QA",
		"PreProd" => "PreProd",
		"Production" => "Production",
		"UAT" => "UAT"
	}

  	desc "Creating Build for QA"
  	lane :create_qa_build do
  		scheme = "QA" #Scheme name
  		set_info_plist_value(path: "./XXXXPATHXXXX/Info.plist", key: "CFBundleDisplayName", value: "XXXXAPP NAMEXXXX QA")
    	gym(
    		output_directory: "./fastlane/builds/#{paths[scheme]}/#{build}",
    		scheme: scheme
    	)

      	distribute_on_crashlytics(scheme: scheme, build: build)

      	save_to_drive(scheme: scheme, build: build)
  	end

  	desc "Create Build for PreProd"
  	lane :create_preprod_build do
  		scheme = "PreProd"
  		set_info_plist_value(path: "./XXXXPATHXXXX/Info.plist", key: "CFBundleDisplayName", value: "XXXXAPP NAMEXXXX PreProd")
    	gym(
    		output_directory: "./fastlane/builds/#{paths[scheme]}/#{build}",
    		scheme: scheme
    	)
    	
    	distribute_on_crashlytics(scheme: scheme, build: build)

    	save_to_drive(scheme: scheme, build: build)
  	end

  	desc "Create Build for Production"
  	lane :create_production_build do
  		scheme = "Production"
  		set_info_plist_value(path: "./XXXXPATHXXXX/Info.plist", key: "CFBundleDisplayName", value: "XXXXAPP NAMEXXXX")
    	gym(
    		output_directory: "./fastlane/builds/#{paths[scheme]}/#{build}",
    		scheme: scheme
    	)

    	save_to_drive(scheme: scheme, build: build)
  	end

  	desc "Create Build for UAT"
  	lane :create_uat_build do
  		scheme = "UAT"
  		set_info_plist_value(path: "./XXXXPATHXXXX/Info.plist", key: "CFBundleDisplayName", value: "XXXXAPP NAMEXXXX UAT")
    	gym(
    		output_directory: "./fastlane/builds/#{paths[scheme]}/#{build}",
    		scheme: scheme
    	)

    	save_to_drive(scheme: scheme, build: build)
  	end
	
	desc "Distribute IPAs"
  	lane :distribute_on_crashlytics do |options| 
  		crashlytics(
			crashlytics_path: "./Pods/Crashlytics/submit", # path to your Crashlytics submit binary.
	    	api_token: "XXXX API TOKEN XXXX",
	        build_secret: "XXXX BUILD SECRATE XXXX",
	        ipa_path: "./fastlane/builds/#{paths[options[:scheme]]}/#{options[:build]}/app_name.ipa", #PATH TO EXPORTED IPA
	        notes_path: "./ChangeLog.txt", #PATH TO RELESE NOTES
	        emails: "yashxxxx@gmail.com", #TESTER'S EMAILS
	        notifications: true
        )
  	end

  	desc "Save IPA to Google Drive"
  	lane :save_to_drive  do |options|
  		createdFolder = create_google_drive_folder(
			drive_keyfile: "./fastlane/config.json",
			folder_id: folderIds[options[:scheme]],
			folder_title: "#{options[:build]}"
		)

  	upload_to_google_drive(
		  drive_keyfile: "./fastlane/config.json",
		  service_account: false,
		  folder_id: "#{createdFolder.id}",
		  upload_files: ["./fastlane/builds/#{paths[options[:scheme]]}/#{options[:build]}/app_name.ipa"]
		)
  	end

end
