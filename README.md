# Spotify Widget

#### 1. Create a new [Spotify Application](https://developer.spotify.com/dashboard/applications)

#### 2. Navigate to <u>_Edit Settings_ --> _Redirect URIs_</u>

- Add the following URL:

- http://localhost/callback/

#### 3. Navigate back to the <u>_Overview_</u> page

- Take note of the following:

- Client ID

- Client Secret

#### 4. Navigate to the following URL:

```http
https://accounts.spotify.com/authorize?client_id={{CLIENT_ID}}&response_type=code&scope=user-read-currently-playing,user-read-recently-played&redirect_uri=http://localhost/callback/
```

#### 5. After logging save the `{{CODE}}`

```http
http://localhost/callback/?code={{CODE}}
```

#### 6. Base64 Encode your client id and client secret with the command below:

```sh
echo -n {{CLIENT_ID}}:{{CLIENT_SECRET}} | base64
```

#### 7. Run the following command:

```sh
curl -X POST -H "Content-Type: application/x-www-form-urlencoded" -H "Authorization: Basic {{BASE64}}=" -d "grant_type=authorization_code&redirect_uri=http://localhost/callback/?code={{CODE}}" https://accounts.spotify.com/api/token
```

#### 8. Save the output and take not of the following:

- **Access_Token**
- **Refresh Token**

#### 9. Fork this repo using the following command:

```sh
git clone https://github.com/jodisfields/spotify-api.git
git remote add fork https://github.com/{{YOUR_USERNAME}}/{{YOUR_REPO}}.git
git push fork master
```

#### 10. Register for an account on [Vercel](https://vercel.com/) and link your new repo with a project

<details>
  <summary>Click to expand</summary>
  <img style="display: block;-webkit-user-select: none;margin: auto;cursor: zoom-in;background-color: hsl(0, 0%, 70%);transition: background-color 300ms;" src="https://i.imgur.com/ZeeEpHY.png/" width="auto%" height="auto">
</details>

#### 11. Add the following environment variables to your project

```ini
SPOTIFY_REFRESH_TOKEN: {{REFRESH_TOKEN}}
SPOTIFY_CLIENT_ID: {{CLIENT_ID}}
SPOTIFY_SECRET_ID: {{SECRET_ID}}
```

#### 12. Deploy your project and enjoy!

![Spotify](https://jodisfields-spotify.vercel.app/api/spotify)

---

## Customization

### Hide the EQ bar

Remove the `#` in front of `contentBar` in [line 81](https://github.com/novatorem/novatorem/blob/98ba4a8489ad86f5f73e95088e620e8859d28e71/api/spotify.py#L81) of current master, then the EQ bar will be hidden when you're in not currently playing anything.

### Status String

Have a string saying either "Vibing to:" or "Last seen playing:".

- Change [`height` to `height + 40`](https://github.com/novatorem/novatorem/blob/5194a689253ee4c89a9d365260d6050923d93dd5/api/templates/spotify.html.j2#L1-L2) (or whatever `margin-top` is set to)
- Uncomment [**.main**'s `margin-top`](https://github.com/novatorem/novatorem/blob/5194a689253ee4c89a9d365260d6050923d93dd5/api/templates/spotify.html.j2#L10)
- Uncomment [currentStatus](https://github.com/novatorem/novatorem/blob/5194a689253ee4c89a9d365260d6050923d93dd5/api/templates/spotify.html.j2#L93)

### Theme Templates

If you want to change the widget theme, you can do so by the changing the `current-theme` property in the `templates.json` file.

Themes:

- `light`
- `dark`

If you wish to customize farther, you can add your own customized `spotify.html.j2` file to the templates folder, and add the theme and file name to the `templates` dictionary in the `templates.json` file.

#### Colour

You can customize the appearance of your `Card` however you wish with URL params.

#### Common Options:

- `background_color` - Card's background color _(hex color)_ without `#`
- `border_color` - Card border color _(hex color)_ without `#`

Use `/?background_color=8b0000&border_color=ffffff` parameter like so:
&nbsp; <br> [![Spotify](https://jodisfields-spotify.vercel.app/api/spotify?background_color=0d1117&border_color=ffffff)]()

#### Spotify Logo

You can add the spotify logo by removing the commented out code, seen below:

```html
<a href="{{songURI}}" class="spotify-logo">
  <svg role="img" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg">
    <title>Spotify</title>
    <path
      d="M12 0C5.4 0 0 5.4 0 12s5.4 12 12 12 12-5.4 12-12S18.66 0 12 0zm5.521 17.34c-.24.359-.66.48-1.021.24-2.82-1.74-6.36-2.101-10.561-1.141-.418.122-.779-.179-.899-.539-.12-.421.18-.78.54-.9 4.56-1.021 8.52-.6 11.64 1.32.42.18.479.659.301 1.02zm1.44-3.3c-.301.42-.841.6-1.262.3-3.239-1.98-8.159-2.58-11.939-1.38-.479.12-1.02-.12-1.14-.6-.12-.48.12-1.021.6-1.141C9.6 9.9 15 10.561 18.72 12.84c.361.181.54.78.241 1.2zm.12-3.36C15.24 8.4 8.82 8.16 5.16 9.301c-.6.179-1.2-.181-1.38-.721-.18-.601.18-1.2.72-1.381 4.26-1.26 11.28-1.02 15.721 1.621.539.3.719 1.02.419 1.56-.299.421-1.02.599-1.559.3z"
    />
  </svg>
</a>
```

### Debugging

If you have issues setting up, try following this [guide](https://youtu.be/n6d4KHSKqGk?t=615).

Followed the guide and still having problems?
Try checking out the functions tab in vercel, linked as:
`https://vercel.com/{name}/spotify/{build}/functions`

<details><summary>Which looks like-</summary>

![image](https://user-images.githubusercontent.com/16753077/91338931-b0326680-e7a3-11ea-8178-5499e0e73250.png)

</details><br>

You will see a log there, and most issues can be resolved by ensuring you have the correct variables from setup.
