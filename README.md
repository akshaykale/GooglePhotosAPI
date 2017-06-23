# GooglePhotosAPI
Wrapper of picasa web API. Get all albums, photos in google photos and google drive.

## API

`base-url : "https://google-photos-api.herokuapp.com/"`

### Fetch all Albums:
`end point: /getAllAlbums`

```
curl -X POST \
  BASE-URL/getAllAlbums \
  -H 'content-type: application/json' \
  -d '{
  "access_token":"xxxxxxxxxxxxxxxxxxxxxxxxx",
  "userid":"user_email_address"
}'
```

### Fetch all photos in an albums:
`end point: /getAllPhotosInAlbum`

```
curl -X POST \
  BASE-URL/getAllPhotosInAlbum \
  -H 'content-type: application/json' \
  -d '{
  "access_token":"xxxxxxxxxxxxxxxxxxxxxxxxx",
  "userid":"user_email_address",
  "album_id" "1212121212121212121",
  "start_index":"1",
  "max_results":"1000"
}'
```

### Fetch all photos in all albums:
`end point: /getAllPhotos`

```
curl -X POST \
  BASE-URL/getAllPhotosInAlbum \
  -H 'content-type: application/json' \
  -d '{
  "access_token":"xxxxxxxxxxxxxxxxxxxxxxxxx",
  "userid":"user_email_address"
}'
```


## Get access token

`GoogleSignInOptions`

```
GoogleSignInOptions options = GoogleSignInOptions.Builder(GoogleSignInOptions.DEFAULT_SIGN_IN)
                .requestScopes(new Scope("https://picasaweb.google.com/data/"))
                .requestServerAuthCode("xxxxxxxxxxxx") //client ID
                .requestEmail()
                .requestProfile()
                .build();
```

```
// Build a GoogleApiClient with access to the Google Sign-In API
        GoogleApiClient mGoogleApiClient = new GoogleApiClient.Builder(this)
                .enableAutoManage(this /* FragmentActivity */, this /* OnConnectionFailedListener */)
                .addApi(Auth.GOOGLE_SIGN_IN_API, options)
                .build();
```

```
@Override
    public void onActivityResult(int requestCode, int resultCode, Intent data) {
        super.onActivityResult(requestCode, resultCode, data);

        // Result returned from launching the Intent from GoogleSignInApi.getSignInIntent(...);
        if (requestCode == RC_SIGN_IN) {
            GoogleSignInResult result = Auth.GoogleSignInApi.getSignInResultFromIntent(data);
            if (result.isSuccess()) {
                // Google Sign In was successful
                GoogleSignInAccount account = result.getSignInAccount();

                String serverAuthCode = getServerAuthCode();
                
                //Request Access token and refresh Token
                GoogleTokenResponse tokenResponse =
                    new GoogleAuthorizationCodeTokenRequest(
                            new NetHttpTransport(),
                            new AndroidJsonFactory(),
                            "https://www.googleapis.com/oauth2/v4/token",
                            "xxxxxxxxxxxxxx", //client ID
                            "xxxxxxxxxxxxxxxxxxxxx", //client secret
                            serverAuthCode,
                            "") 
                            .execute();

                String accessToken = tokenResponse.getAccessToken();
                String refreshToken = tokenResponse.getRefreshToken();
                
                
            } else {
                Log.d(DEBUG_LOG,"Failed");
            }
        }
    }
```

