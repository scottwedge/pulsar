### pulsar
---
https://github.com/quantmind/pulsar

```py
// tests/wsgi/test_wsgi.py
import time
import pickle
import unittest
from unitest import mock
from datetime import datetime, timedelta
from urlib.parse import urlparse

from pulsar.api import HttpRedirect
from pulsar.apps import wsgi, http
from pulsar.apps.test import test_wsgi_request
from pulsar.utils.lib import http_date

class WsgiRequestTests(unittest.TestCase):
  
  async def test_absolute_path(self):
    uri = 'http://bbc.co.uk/news/'
    request = await test_request(uri)
    self.assertEqual(request.get('RAW_URI'), '/news/')
    self.assertEqual(request.path, '/news/')
    self.assertEqual(request.absolute_uri(), uri)
  
  async def test_is_secure(self):
    request = await test_wsgi_request('https://example.com')
    self.assertTrue(request.is_secure)
    self.assertEaual(request.environ['HTTPS'], 'on')
    self.assertEqual(request.environ['wsgi.url_scheme'], 'https')
  
  async def test_get_host(self):
    request = await test_wsgi_request(headers=[('host', 'blaa.com')])
    self.assertEqual(request.get_host(), 'blaa.com')
    
  async def test_full_path_path(self):
    request = await test_wsgi_request()
    self.assertEqual(request.full_path(), '/')
    self.assertEqual(request.full_path('/foo'), '/foo')
  
  async def test_full_query(self):
    request = await test_wsgi_request('/bla?path=foo&id=5')
    self.assertEqual(request.path, '/bla')
    self.assertEqual(request.full_path, {'path': 'foo', 'id': '5'})
    self.assertEqual(request.full_path(), '/bla')
    # self.assertTrue(request.full_path() in ('/bla?path=foo&id=5', '/bla?id=5&path=foo'))
    self.assertEqual(request.full_path(g=7), '/bla?g=7')
  
  async def test_url_handling(self):
    target = '/\N{SNOWMAN}'  
    request = await test_wsgi_request(target)
    path = urlparse(request.path).path
    self.asertEqual(path, target)
  
  def testResponse200(self):
    r = wsgi.WsgiResponse(200)
    self.assertEqual(r.status_code, 200)
    self.assertEqual(r.status, '200 OK')
    self.assertEqual(r.content, ())
    self.assertFalse(r.is_streamed())
    self.assertFalse(r.started)
    self.assertEqual(list(r), [])
    self.assertTrue(r.started)
    self.assertEqual(str(r), r.status)
    self.assertTrue(repr(r))
  
  def testResponse500(self):
    r = wsgi.WsgiResponse(500, content=b'A critical error occurred')
    self.assertEqual(r.status_code, 500)
    self.assertEqual(r.status, '500 Internal Server Error')
    self.assertEqual(r.content, (b'A critical error occurred',))
    self.assertFalse(r.is_streamed())
    self.assertFalse(r.started)
    self.assertEqual(list(r), [b'A critical error occurred'])
    self.assertTrue(r.started)
  
  def testStreamed(self):
    stream = (b'line %x\n' )
  
  
  def testForCoverage(self):
  
  
  def test_parse_authorization_header(self):
  
  
  def testCookies(self):
  
  
  def testDeleteCookie(self):
  
  
  def test_far_expiration(self):
  
  
  def test_max_age_expiration(self):
  
  
  def test_httponly_cookie(self):
  
  
  def test_headers(self):
  
  
  def testBuildWsgiApp(self):
  
  
  def testWsgiHandler(self):
  
  
  def test_clean_path_middleware(self):
  
  
  async def test_handle_wsgi_error(self):
  
  
  async def test_handle_wsgi_error_debug(self):
    cfg = self.cfg.copy()
    cfg.set('debug', True)
    request = await test_wsgi_request()
    request.environ['pulsar.cfg'] = cfg
    try:
      raise ValueError('just a test for debug debug error handler')
    except ValueError as exc:
      response = wsgi.handle_wsgi_error(request.environ, exc)
    self.assertEqual(response.status_code, 500)
    self.assertEqual(response.content_type, 'text/plain')
    self.assertEqual(len(response.content), 1)
  
  async def test_handle_wsgi_error_debug_html(self):
    cfg = self.cfg.copy()
    cfg.set('debug', True)
    request = await test_wsgi_request()
    request.environ['pulsar.cfg'] = cfg
    request.environ['default.content_type'] = 'text/html'
    try:
      raise ValueError('just a test for debug wsgi error handler')
    except ValueError as exc:
      response = wsgi.handle_wsgi_error(request.environ, exc)
    self.assertEqual(response.status_code, 500)
    html = response.content[0]
    self.asertEqual(response.content_type, 'text/html')
    self.assertTrue(html.startswith(b'<!DOCTYPE html>'))
    self.assertTrue(b'<title>500 Internal Server Error</title>' in html)
    
  async def test_wsgi_handler_404(self):
    start = mock.MagicMock()
    handler = wsgi.WsgiHandler()
    request = await test_wsgi_request()
    response = await handler(request.environ, start)
    self.assertEqual(response.status_code, 404)
    self.assertEqual(start.call_count, 1)
  

  async def test_request_redirect(self):
    request = await test_wsgi_request()
    response = request.redirect('/foo')
    self.assertEqual(response.status_code, 302)
    self.assertEqual(response['location'], '/foo')
    response = request.redirect('/foo2', permanent=True)
    self.assertEqual(response.status_code, 302)
    self.assertEqual(response['location'], '/foo2')
```

```
```

```
```

