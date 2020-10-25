## Springboot通过mapperId反射调用dao层的mapper动态代理实现

~~~java
import com.ooboou.point.demo.config.MyInvocationHandler;
import com.ooboou.point.demo.dao.ZxRoleDAO;
import com.ooboou.point.demo.domain.ZxRole;
import com.ooboou.point.demo.localservice.ZxRoleService;
import org.apache.ibatis.session.SqlSession;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

import java.lang.reflect.InvocationTargetException;
import java.lang.reflect.Method;
import java.lang.reflect.Proxy;
import java.util.List;

@Service("zxRoleService")
@SuppressWarnings("unchecked")
public class ZxRoleServiceImpl implements ZxRoleService {

    @Autowired
    private SqlSession sqlSession;

    @Override
    public ZxRole getZxRoleByID(Integer id)
            throws ClassNotFoundException,
            NoSuchMethodException,
            InvocationTargetException,
            IllegalAccessException {

        Class interfaceDao = Class.forName("com.ooboou.point.demo.dao.ZxRoleDAO");//这里要写全类名
        Class paramClass = Class.forName("java.lang.Integer");//方法的入参

        Method method = interfaceDao.getMethod("getZxRoleByID", paramClass);
        Object obj = method.invoke(sqlSession.getMapper(interfaceDao), id);
        return (ZxRole) obj;
    }

}
~~~

