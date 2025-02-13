# Updating to a new version of Google's libphonenumber

## Prepare the repo with required version and Annatel's changes

- If you don't already have it, get Annatel's fork of libphonenumber : `git clone git@github.com:annatel/libphonenumber.git`
- cd libphonenumber/
- If you didn't already add it, add Google's version as upstream : `git remote add upstream git@github.com:google/libphonenumber.git`
- fetch all changes and tags : `git fetch --all --tag`
- create a local branch for your new version basing it ont the tag from Google that you're interested in  (e.g. vL.AS.T): `git checkout -b antl-vL.AS.T vL.AS.T`
- find what was the latest version from Annatel (e.g. vP.RE.V) : `git branch -r | grep antl`
- apply Annatel's diff to your new branch : `git diff vP.RE.V..antl-vP.RE.V | git apply -`
  If it doesn't work automatically you'll have to do it manually, try that alternative procedure :
  - `git diff vP.RE.V..antl-P.RE.V > patch.diff`
  - edit patch.diff, save all hunks that are not problematic in a new file named p1.diff and the problematic ones in p2.diff
  - `git apply p1.diff`
  - manually apply p2.diff (probably changes regarding `resources/PhoneNumberMetadata.xml` ?)
- check that the changes look good; you should see a small number (~4) of new or changed files: `git status`
- if you need to do other changes, fix previous changes, etc, do it now
- add the files to a new commit : `git add .github/workflows/release.yml Earthfile resources/PhoneNumberMetadata.xml ... your files`
- commit : `git commit -m "Annatel internal changes"`
- push your new branch : `git push --set-upstream origin antl-vL.AS.T`

## Release the new version

- go to https://github.com/annatel/libphonenumber/
- In the right colmn, click on "Releases"
- click on "Draft a new release"
- click on the "Choose a tag" dropdown
- in the "Find or create a new tag" field enter a name for the tag of the new release, (e.g. vL.AS.T-antl-0.42.0)
- click on the "Target: master" dropdown
- in the "Branches" tab, chose the branch you created earlier (e.g. antl-vL.AS.T)
- click on the "Publish release" button
- you should now see the page of the release you just created but the assets.zip file is not there yet in the "Assets" files list
- wait approximately 5 minutes (You can check if it is finished in the "Actions" tab)
- reload the page of the release
- you should now see the assets.zip file in the "Assets" file list
- done ✅

## Check the new version

You should update antl_phonenumber build environment to use the new version and check that all its tests still pass!

## Use the new version

You can now update the Earthfile of your projects with the url of the new assets.zip file.

Be carefull, it often appears twice in internal projects Earthfiles.

