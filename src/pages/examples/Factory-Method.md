---
layout: '../../layouts/Layout.astro'
pattern: 'iterator'
---


# Factory Method
---
Also known as Virtual Constructor, The *Factory Method* pattern is a [creational design pattern](https://www.javatpoint.com/creational-design-patterns) used to create objects without specifying the concrete class of object to be created. Instead of instantiating objects directly with the new operator, a method (the *"Factory Method"*) is defined in an interface or base class that subclasses must implement. Each subclass can provide its own implementation of the *Factory Method* to create concrete objects.

The *Factory Method* is used when you have a class hierarchy and want to delegate the responsibility of object creation to subclasses. This promotes the "Open/Closed" principle, which states that classes should be open for extension but closed for modification. In other words, you can add new concrete classes (extensions) without having to modify the code that uses the *Factory Method*.

## Structure
---

[![structure.png](https://i.postimg.cc/BbVxf1w4/structure.png)](https://postimg.cc/ctfKQ6Sb)

1. The Product declares the interface, which is common to all objects that can be produced by the creator and its subclasses.

2. Concrete Products are different implementations of the product interface.

3. Concrete Creators override the base *factory method* so it returns a different type of product.
Note that the *factory method* doesn’t have to create new instances all the time. It can also return existing objects from a cache, an object pool, or another source.

 4. The Creator class declares the *factory method* that returns new product objects. It’s important that the return type of this method matches the product interface.
You can declare the *factory method* as abstract to force all subclasses to implement their own versions of the method. As an alternative, the base *factory method* can return some default product type.

>   Note, despite its name, product creation is not the primary responsibility of the creator. Usually, the creator class already has some core business logic related to products. The *factory method* helps to decouple this logic from the concrete product classes. Here is an analogy: a large software development company can have a training department for programmers. However, the primary function of the company as a whole is still writing code, not producing programmers.

If you want to learn more about this software design pattern visit: [*Factory Method*](https://refactoring.guru/design-patterns/factory-method)

## H files
----
### What are header files for in C++?

A file saved with h file extension is a header file used in C/C++ files to include the declaration of variables, constants, and functions. These are referred by the C++ implementation files that contain the actual implementation of these functions. A .h header file can also include additional information such as Macro definitions. These header files are referenced in the C/C++ files using the `#include` directive.

A new C++ project usually contains a special header file by the name stdafx.h file that is the starting point for all compilation chains and all the header files can be included in this single file. A .h file can be opened with any text editor, Eclipse IDE, Microsoft Visual Studio IDE, Borland C++ compiler and a lot other applications.

## H File Format 
A .h file is plain text file that has its own rules for defining the syntax. Header files can contain the following information.

`Variables` - In case of Object Oriented Programming (OOP), a class header file contains definitions of all class level variables that are accessible across the implementation source code files `Methods Declaration` - All the methods declarations are included in the .h header files to be accessible across multiple implementation files. `Non-Inline Function Definitions` - Header files can also contain definitions of a non-inline methods. `Message Maps` - A header file can also contain message maps in case of an MFC source code implementation. In such case, the message maps are linked to the functionality implementation which is linked to UI elements such as button, checkbox, radio buttons, etc.

[Learn more about header files.](https://docs.fileformat.com/programming/h/#:~:text=A%20file%20saved%20with%20h,actual%20implementation%20of%20these%20functions.)

### Are head-file in C++ the same as abstract class/interface in Java?

C or C++ declarations are subject to the One Definition Rule. A compiled program can contain only one implementation for each declared function or global variable (unless it is declared as inline, in which all definitions MUST be equivalent). So for one function declaration in a header, I write one function definition in a .c or .cpp file. If I want to use a different definition, I need to recompile the code.

Java interfaces (or C++ classes with pure virtual methods) also declare a kind of “interface”, but it is very different: one class can implement many interfaces, and one interface can be implemented by many classes. The latter point is very important: I can write code that only calls methods on an object through an interface. Later, I can provide objects that conform to this interface to that code, and the code will still work! I can choose multiple implementations at run time, without having to recompile the code. This provides a lot of flexibility, e.g. for dependency injection.

Fundamentally, headers and interfaces work very differently. Headers contain forward declarations that are resolved later, but still during compilation. An interface (or non-final class) defines virtual methods that are resolved at run time through dynamic dispatch.

They also mean different concepts. Mechanisms like headers and public/private access modifiers in classes allow us to implement abstract data types (ADTs). ADTs encapsulate implementation details of some data structure, and only allow external code to access to those structures through public functions or methods. In contrast, interfaces and virtual methods allow us to implement objects. One interface can be implemented by many different objects. This ability of objects of one common interface to behave differently is also called polymorphism.

ADTs and objects both have something to do with “encapsulation”, but they do that very differently. They should be understood as complementary concepts.

However, there has been significant interest in object-oriented solutions. E.g. the Design Patterns book investigates how applying object-oriented techniques like dynamic dispatch can introduce enough flexibility into a software system to solve some interesting problems. As a random example, the Composite Pattern allows us to treat collections of items equivalently to a single item. How? both the collection and single items implement the same interface. This wouldn't be possible just with header files.

Fragment taken from [StackExchange](https://softwareengineering.stackexchange.com/questions/370215/difference-between-header-files-and-interfaces#:~:text=Fundamentally%2C%20headers%20and%20interface%20s,They%20also%20mean%20different%20concepts.).

## Real application example
### TensorFlow
---

[![68747470733a2f2f7777772e74656e736f72666c6f772e6f72672f696d616765732f74665f6c6f676f5f686f72697a6f6e74.png](https://i.postimg.cc/qMDQ8RsV/68747470733a2f2f7777772e74656e736f72666c6f772e6f72672f696d616765732f74665f6c6f676f5f686f72697a6f6e74.png)](https://postimg.cc/mtMQ4Tp8)

TensorFlow is an end-to-end open source platform for machine learning. It has a comprehensive, flexible ecosystem of tools, libraries, and community resources that lets researchers push the state-of-the-art in ML and developers easily build and deploy ML-powered applications.

TensorFlow was originally developed by researchers and engineers working within the Machine Intelligence team at Google Brain to conduct research in machine learning and neural networks. However, the framework is versatile enough to be used in other areas as well.

TensorFlow provides stable Python and C++ APIs, as well as a non-guaranteed backward compatible API for other languages.

#### If you want to learn more about TensorFlow and machine learning, read the following documentation:
- [How to Start Learning Machine Learning?](https://www.geeksforgeeks.org/how-to-start-learning-machine-learning/)
- [First steps in Machine Learning with TensorFlow](https://coderslink.com/talento/blog/primeros-pasos-en-machine-learning-con-tensorflow/)
- [TensorFlow 2 quickstart for beginners](https://www.tensorflow.org/tutorials/quickstart/beginner)


### How is  *Factory Method* implemented in TensorFlow?

---

Regarding the implementation of the design pattern in TensorFlow, you can observe the header file `device_factory.h` the function of this is to provide infrastructure for registering, creating, and managing different types of devices (such as CPU and GPU) that can be used in the context of machine learning with TensorFlow.


1. `DeviceFactory` is a base class that defines the interface for device factories. Device factories are responsible for creating instances of specific devices (e.g., CPU or GPU).

2. The code provides static methods for registering device factories. This allows different device implementations to be registered in the system and used as needed.

3. `Register` allows concrete classes (device factories) to register and create device instances without knowing the specific implementation details. Parameters include the device type, device factory, priority, and whether it's a pluggable device.

4. `GetFactory` is used to obtain a device factory for a specific device type.

5. `AddCpuDevices` and `AddDevices` are used to add devices to the system. `AddCpuDevices` adds CPU devices, and `AddDevices` adds all suitable devices, considering the specific device type properties in the options.

6. `ListAllPhysicalDevices` and `ListPluggablePhysicalDevices` generate lists of available physical and pluggable devices, respectively.

7. `GetAnyDeviceDetails` retrieves details of a specific device from all available device factories.

8. `ListPhysicalDevices` and `GetDeviceDetails` are functions that should be implemented in concrete device factories to list available physical devices and get specific device details, respectively.

9. `CreateDevices` is an abstract function that must be implemented in concrete device factories to create device instances.

10. `DevicePriority` returns the priority of a device type, which can influence the device choice when multiple options are available.

11. `IsPluggableDevice` checks if a device type is a pluggable or first-party device.

If you want to learn more about this proyect visit: [TensorFlow](https://github.com/tensorflow/tensorflow.git)

### device_factory.h
```C++

#ifndef TENSORFLOW_CORE_FRAMEWORK_DEVICE_FACTORY_H_
#define TENSORFLOW_CORE_FRAMEWORK_DEVICE_FACTORY_H_

#include <string>
#include <vector>
#include "absl/base/attributes.h"
#include "tensorflow/core/platform/status.h"
#include "tensorflow/core/platform/types.h"

namespace tensorflow {

class Device;
struct SessionOptions;

class DeviceFactory {
public:
  virtual ~DeviceFactory() {}

  // Registers a device factory for a specific device type.
  static void Register(const std::string& device_type,
                      std::unique_ptr<DeviceFactory> factory, int priority,
                      bool is_pluggable_device);

  // Gets a registered device factory for a device type.
  static DeviceFactory* GetFactory(const std.string& device_type);

  // Appends CPU devices to the devices list.
  static Status AddCpuDevices(const SessionOptions& options,
                              const std::string& name_prefix,
                              std::vector<std::unique_ptr<Device>>* devices);

  // Appends devices to the list, respecting device-specific properties
  // and session options.
  static Status AddDevices(const SessionOptions& options,
                          const std::string& name_prefix,
                          std::vector<std::unique_ptr<Device>>* devices);

  // Creates a new device of a specific type.
  static std::unique_ptr<Device> NewDevice(const string& type,
                                          const SessionOptions& options,
                                          const string& name_prefix);

  // Lists all available physical devices.
  static Status ListAllPhysicalDevices(std::vector<string>* devices);

  // Lists all available pluggable physical devices.
  static Status ListPluggablePhysicalDevices(std::vector<string>* devices);

  // Gets details for a specific device among all available devices.
  static Status GetAnyDeviceDetails(
      int device_index, std::unordered_map<string, string>* details);

  // Interface to list physical devices for a device factory.
  virtual Status ListPhysicalDevices(std::vector<string>* devices) = 0;

  // Gets details for a specific device for a device factory.
  virtual Status GetDeviceDetails(int device_index,
                                  std::unordered_map<string, string>* details) {
    return OkStatus();
  }

  // Creates devices based on session options and a specific name.
  virtual Status CreateDevices(
      const SessionOptions& options, const std::string& name_prefix,
      std::vector<std::unique_ptr<Device>>* devices) = 0;

  // Gets the priority of a device type.
  static int32 DevicePriority(const std::string& device_type);

  // Checks if a device type is pluggable.
  static bool IsPluggableDevice(const std::string& device_type);
};

namespace dfactory {

template <class Factory>
class Registrar {
public:
  // Registers a device factory with a priority for a device type.
  explicit Registrar(const std::string& device_type, int priority = 50) {
    DeviceFactory::Register(device_type, std::make_unique<Factory>(), priority,
                            /*is_pluggable_device*/ false);
  }
};

}  // namespace dfactory

#define REGISTER_LOCAL_DEVICE_FACTORY(device_type, device_factory, ...) \
  INTERNAL_REGISTER_LOCAL_DEVICE_FACTORY(device_type, device_factory,   \
                                        __COUNTER__, ##__VA_ARGS__)

#define INTERNAL_REGISTER_LOCAL_DEVICE_FACTORY(device_type, device_factory, \
                                              ctr, ...)                    \
  static ::tensorflow::dfactory::Registrar<device_factory>                  \
  INTERNAL_REGISTER_LOCAL_DEVICE_FACTORY_NAME(ctr)(device_type, ##__VA_ARGS__)

#define INTERNAL_REGISTER_LOCAL_DEVICE_FACTORY_NAME(ctr) ___##ctr##__object_

}  // namespace tensorflow

#endif  // TENSORFLOW_CORE_FRAMEWORK_DEVICE_FACTORY_H_

```

[Code in the TensorFlow repository](https://github.com/tensorflow/tensorflow/blob/b54c23fa11763d99c671e14037d615a28daa0edc/tensorflow/core/framework/device_factory.h#L4)
## How does it relate to the rest of the system?

Look at the following code that implements `device_factory.h`

### fallback_state.cpp
```c++

#include "tensorflow/core/tfrt/fallback/fallback_state.h"

#include <cstddef>
#include <cstdint>
#include <memory>
#include <utility>
#include <vector>

#include "absl/strings/string_view.h"
#include "absl/strings/strip.h"
#include "tensorflow/core/common_runtime/device_mgr.h"
#include "tensorflow/core/common_runtime/rendezvous_mgr.h"
#include "tensorflow/core/framework/device_attributes.pb.h"
#include "tensorflow/core/framework/device_factory.h" // including device_factory.h
#include "tensorflow/core/framework/types.h"
#include "tensorflow/core/graph/types.h"
#include "tensorflow/core/platform/strcat.h"
#include "tensorflow/core/platform/types.h"
#include "tensorflow/core/public/version.h"
#include "tensorflow/core/tpu/virtual_device.h"

namespace tensorflow {
namespace tfrt_stub {

namespace {

// Function to generate a unique device name based on prefix, type, task ID, and device ID.
string DeviceName(absl::string_view name_prefix, absl::string_view device_type,
                  int32_t task_id, size_t device_id) {
  return strings::StrCat(absl::StripSuffix(name_prefix, "0"), task_id,
                         "/device:", device_type, ":", device_id);
}

// Function to build device attributes using the specified parameters.
DeviceAttributes BuildDeviceAttributes(absl::string_view name_prefix,
                                       const char *device_type, int32_t task_id,
                                       size_t device_id) {
  const DeviceAttributes attrs = Device::BuildDeviceAttributes(
      DeviceName(name_prefix, device_type, task_id, device_id),
      DeviceType(device_type), Bytes(16ULL << 30), DeviceLocality(),
      strings::StrCat("device: ", device_type, " device"));
  return attrs;
}

}  // namespace

// Function to create FallbackState instance with provided session options and function definition library.
StatusOr<std::unique_ptr<FallbackState>> FallbackState::Create(
    const SessionOptions &session_options,
    const tensorflow::FunctionDefLibrary &fdef_lib) {
  // Create devices.
  std::vector<std::unique_ptr<Device>> devices;
  TF_RETURN_IF_ERROR(DeviceFactory::AddDevices(
      session_options, "/job:localhost/replica:0/task:0", &devices));

  return std::make_unique<FallbackState>(session_options, std::move(devices),
                                         fdef_lib);
}

// Function to create FallbackState instance with CPU devices.
StatusOr<std::unique_ptr<FallbackState>> FallbackState::CreateWithCpuDevice(
    const SessionOptions &session_options,
    const tensorflow::FunctionDefLibrary &fdef_lib) {
  // Create CPU devices.
  std::vector<std::unique_ptr<Device>> devices;
  TF_RETURN_IF_ERROR(DeviceFactory::AddCpuDevices(
      session_options, "/job:localhost/replica:0/task:0", &devices));

  return std::make_unique<FallbackState>(session_options, std::move(devices),
                                         fdef_lib);
}

// Function to create FallbackState instance with a mock GPU device.
StatusOr<std::unique_ptr<FallbackState>> FallbackState::CreateWithMockGpuDevice(
    const SessionOptions &session_options,
    const tensorflow::FunctionDefLibrary &fdef_lib) {
  // Create CPU devices.
  std::vector<std::unique_ptr<Device>> devices;
  TF_RETURN_IF_ERROR(DeviceFactory::AddCpuDevices(
      session_options, "/job:localhost/replica:0/task:0", &devices));

  // Create a mock GPU device.
  auto device_attrs =
      BuildDeviceAttributes("/job:localhost/replica:0/task:0", "GPU", 0, 0);
  devices.push_back(
      std::make_unique<VirtualDevice>(session_options.env, device_attrs));

  return std::make_unique<FallbackState>(session_options, std::move(devices),
                                         fdef_lib);
}

// Constructor for FallbackState.
FallbackState::FallbackState(const SessionOptions &session_options,
                             std::vector<std::unique_ptr<Device>> devices,
                             const tensorflow::FunctionDefLibrary &fdef_lib)
    : session_options_(session_options),
      device_manager_(std::move(devices)),
      func_lib_def_(OpRegistry::Global(), fdef_lib),
      pflr_(&device_manager_, session_options.env, &session_options.config,
            TF_GRAPH_DEF_VERSION, &func_lib_def_,
            session_options.config.graph_options().optimizer_options(),
            /*thread_pool=*/nullptr, /*parent=*/nullptr,
            /*session_metadata=*/nullptr,
            Rendezvous::Factory{[](const int64, const DeviceMgr *device_mgr,
                                   tsl::core::RefCountPtr<Rendezvous> *r) {
              *r = tsl::core::RefCountPtr<Rendezvous>(
                  new IntraProcessRendezvous(device_mgr));
              return OkStatus();
            }}) {
  for (auto *d : device_manager_.ListDevices()) {
    device_set_.AddDevice(d);
  }

  // Set the client device for feed and fetch tensors.
  device_set_.set_client_device(device_manager_.HostCPU());
}

// Function to create GraphExecutionState from GraphDef.
StatusOr<std::unique_ptr<GraphExecutionState>>
FallbackState::CreateGraphExecutionState(GraphDef graph_def,
                                         bool run_placer) const {
  // Create GraphExecutionState with preprocessed graph including device information.
  GraphExecutionStateOptions options;
  options.device_set = &device_set_;
  options.session_options = &session_options_;
  options.session_handle = "tfrt_fallback_handle";
  options.run_placer = run_placer;

  std::unique_ptr<GraphExecutionState> execution_state;
  TF_RETURN_IF_ERROR(GraphExecutionState::MakeForBaseGraph(
      std::move(graph_def), options, &execution_state));
  return execution_state;
}

// Function to add a FunctionDef to the function definition library.
Status FallbackState::AddFunctionDef(const FunctionDef &func_def) {
  return func_lib_def_.AddFunctionDef(func_def);
}

}  // namespace tfrt_stub
}  // namespace tensorflow


```
### How to implement device_factory.h?

This class uses functionalities from the header file `device_factory.h` to create instances of devices and manage the execution state.

Here is a description of the most relevant sections of the source file:

1. **Header inclusions:**
   ```c++
   #include "tensorflow/core/framework/device_factory.h"
   ```
   The `device_factory.h` header file is included, providing the definitions of classes and functions used in this source file.

2. **Namespaces:**
   ```c++
   namespace tensorflow {
   namespace tfrt_stub {
   ```
   The code is defined within the `tensorflow` and `tfrt_stub` namespaces.

3. **Helper functions:**
   A function named `DeviceName` and another named `BuildDeviceAttributes` are defined. These functions appear to be related to creating device names and attributes.

4. **`FallbackState` Class:**
   The `FallbackState` class is implemented with functions for creating instances of devices and managing the execution state. Notable functions include `Create`, `CreateWithCpuDevice`, and `CreateWithMockGpuDevice`, which create instances of `FallbackState` with different device configurations.

5. **Implementation of `device_factory.h` functions:**
   Functions from `DeviceFactory` are used to add devices (`AddDevices` and `AddCpuDevices`) and obtain information about available devices.

6. **Usage of other classes and functions:**
   Additional TensorFlow classes and functions are used, such as `Device`, `FunctionDefLibrary`, `GraphExecutionState`, etc.

In summary, this source file implements the logic for creating instances of the `FallbackState` class, which utilizes functionalities provided by the "device_factory.h" header file to work with devices in TensorFlow. The `Create`, `CreateWithCpuDevice`, and `CreateWithMockGpuDevice` functions are factories that create instances of `FallbackState` with different device configurations.

## Relationship with SOLID and OCP
---
The Factory Method pattern primarily adheres to the SOLID principle known as the Single Responsibility Principle (SRP), although it may relate to other principles depending on its specific implementation.

Single Responsibility Principle (SRP): The Factory Method promotes compliance with the SRP by separating the responsibility of object creation into a specific class (the Factory Method) rather than having this responsibility distributed throughout the client code. Each subclass implementing the Factory Method has the sole responsibility of creating instances of a specific type of object. This facilitates code management and maintenance by focusing each class on a single task.

While the Factory Method is not directly related to other SOLID principles, its use can indirectly contribute to other principles such as the Open/Closed Principle (OCP) and the Liskov Substitution Principle (LSP) as it enables the extension and replacement of classes without modifying existing code.