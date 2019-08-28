---
description: >-
  This lists out all the headers which the app sends to the Simple server in all
  the requests
---

# Headers sent from the app

<table>
  <thead>
    <tr>
      <th style="text-align:left">Key</th>
      <th style="text-align:left">Description</th>
      <th style="text-align:left">Example Value</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left"><code>X-APP-VERSION</code>
      </td>
      <td style="text-align:left">The build version string of the Android client. This is generally auto-generated
        for every build by the continuous deployment service.</td>
      <td style="text-align:left"><code>2019-08-26-5229</code>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>Accept-Language</code>
      </td>
      <td style="text-align:left">
        <p>The current language selected by the user. Currently, this is the device
          language. In the future, when we build out the in-app language switcher,
          will be the language selected by the user. See the official <a href="https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Accept-Language">docs</a> for
          more information.</p>
        <p></p>
        <p>Note: this is only a hint to the server so that they can select strings
          and set the response language correctly. The response is not verified by
          the app to be in the selected language.</p>
      </td>
      <td style="text-align:left"><code>en-US</code>, <code>hi-IN</code>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>X-TIMEZONE-ID</code>
      </td>
      <td style="text-align:left">The timezone set by the user on the device. Will be a standard timezone
        ID.</td>
      <td style="text-align:left"><code>Asia/Kolkata</code>,<code>Etc/GMT-14</code>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>X-TIMEZONE-OFFSET</code>
      </td>
      <td style="text-align:left">
        <p>This will be the raw offset (in seconds) that the current user is from
          UTC. This value will be <b>inclusive</b> of adjustments like <a href="https://en.wikipedia.org/wiki/Daylight_saving_time">DST</a>.</p>
        <p></p>
        <p>This is what the server should use if the server needs to find the calendar
          date a request is being sent from from UTC timestamps.</p>
      </td>
      <td style="text-align:left"><code>3600</code>,<code>-3600</code>
      </td>
    </tr>
  </tbody>
</table>