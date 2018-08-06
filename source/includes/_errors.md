# Errors

<aside class = 'notice'>
If the response was unsuccessful, the response will have an extra field named <code>error_reason</code>, that is a human-parsable string that offers more granular insight as to what caused the error. The <code>error_reason</code>s are shown below each response code in the table below.
</aside>

The GradeBook API uses the following error codes:

<table style = 'width: 50%'>
    <tr style = 'font-weight: 700; width: 100%'>
      <td style = 'text-align: right'>Error Code</td><td style = 'width: 100%'>Meaning</td>
    </tr>
    <tr style = 'width: 100%'>
      <td style = 'text-align: right;'>
        <code>400</code><br/><i>INVALID_INPUT</i>
      </td>
      <td>Bad Request - Input is invalid</td>
    </tr>
    <tr style = 'width: 100%'>
      <td style = 'text-align: right;'>
        <code>401</code><br/><i>INVALID_API_KEY</i>
      </td>
      <td>Unauthorized - API key is invalid</td>
    </tr>
    <tr style = 'width: 100%'>
      <td style = 'text-align: right;'>
        <code>401</code><br/><i>INVALID_CREDENTIALS</i>
      </td>
      <td>Unauthorized - The login information for the user is not recognized by Synergy's servers.</td>
    </tr>
    <tr style = 'width: 100%'>
      <td style = 'text-align: right;'>
        <code>401</code><br/><i>INVALID_COOKIES_GB</i>
      </td>
      <td>Unauthorized - The cookies to authenticate the user into GradeBook's servers were invalid</td>
    </tr>
    <tr style = 'width: 100%'>
      <td style = 'text-align: right;'>
        <code>401</code><br/><i>INVALID_COOKIES_SYNERGY</i>
      </td>
      <td>Unauthorized - The cookies stored in GradeBook's database to authenticate into Synergy's servers were invalid. This is likely because they have expired.</td>
    </tr>
    <tr style = 'width: 100%'>
      <td style = 'text-align: right;'>
        <code>403</code><br/><i>NO_ACCOUNT</i>
      </td>
      <td>Forbidden - Although the inputs were acceptable to the server, the user has not created an account on GradeBook.</td>
    </tr>
    <tr style = 'width: 100%'>
      <td style = 'text-align: right;'>
        <code>404</code><br/><i>NOT_FOUND</i>
      </td>
      <td>Not Found - The requested resource was not found. Check your query parameters to ensure all arguments were within valid bounds.</td>
    </tr>
    <tr style = 'width: 100%'>
      <td style = 'text-align: right;'>
        <code>405</code><br/><i>NOT_GET</i>
      </td>
      <td>Method Not Allowed - The endpoint uses the <code>GET</code> HTTP protocol.</td>
    </tr>
    <tr style = 'width: 100%'>
      <td style = 'text-align: right;'>
        <code>405</code><br/><i>NOT_POST</i>
      </td>
      <td>Method Not Allowed - The endpoint uses the <code>POST</code> HTTP protocol.</td>
    </tr>
</table>