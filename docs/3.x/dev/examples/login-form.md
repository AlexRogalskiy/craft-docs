# Login Form

If you need to login a user from the front-end of your site, you can do so with the following code:

```twig
<form method="post" accept-charset="UTF-8">
    {{ csrfInput() }}
    {{ actionInput('users/login') }}

    <label for="loginName">Username or email</label>
    <input id="loginName" type="text" name="loginName"
        value="{{ craft.app.user.rememberedUsername }}">

    <label for="password">Password</label>
    <input id="password" type="password" name="password">

    <label>
        <input type="checkbox" name="rememberMe" value="1">
        Remember me
    </label>

    <input type="submit" value="Login">

    {% if errorMessage is defined %}
        <p>{{ errorMessage }}</p>
    {% endif %}
</form>

<p><a href="{{ url('forgotpassword') }}">Forgot your password?</a></p>
```

`craft.app.user.returnUrl` is set to the original URL that included the `{% requireLogin %}` tag that initiated the redirect to this login form.

By default, users will be redirected based on your `postLoginRedirect` config setting value after logging in. You can override that within your login form using a `redirect` param:

```twig
{{ redirectInput('some/custom/path') }}
```

