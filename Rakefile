#
# Copyright:: Copyright (c) 2015 Chef Software, Inc.
# License:: Apache License, Version 2.0
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#     http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.
#

require 'rake'

SOURCE = File.join(File.dirname(__FILE__), "MAINTAINERS.toml")
TARGET = File.join(File.dirname(__FILE__), "MAINTAINERS.md")

begin
  require 'tomlrb'
  task :default => :generate

  namespace :maintainers do
    desc "Generate MarkDown version of MAINTAINERS file"
    task :generate do
      maintainers = Tomlrb.load_file SOURCE
      out = "<!-- This is a generated file. Please do not edit directly -->\n\n"
      out << "<!-- Modify MAINTAINERS.toml and run `rake maintainers:generate` to regenerate. -->\n\n"
      out << "# " + maintainers["Preamble"]["title"] + "\n\n"
      out <<  maintainers["Preamble"]["text"] + "\n"
      out << components(maintainers["people"], maintainers["Org"]["Components"])
      File.open(TARGET, "w") { |fn|
        fn.write out
      }
    end
  end

  def components(list, cmp)
    out = "## " + cmp.delete("title") + "\n\n"
    out << cmp.delete("text") + "\n" if cmp.has_key?("text")
     if cmp.has_key?("lieutenant")
       out << "### Lieutenant\n\n"
       out << person(list, cmp.delete("lieutenant")) + "\n\n"
     end
    out << maintainers(list, cmp.delete("maintainers")) + "\n" if cmp.has_key?("maintainers")
    cmp.delete("paths")
    cmp.each {|k,v| out << components(list, v) }
    out
  end

  def maintainers(list, people)
    o = "### Maintainers\n\n"
    people.each do |p|
      o << person(list, p) + "\n"
    end
    o
  end

  def person(list, person)
    if list[person].has_key?("GitHub")
      out = "* [#{list[person]["Name"]}](https://github.com/#{list[person]["GitHub"]})"
    else
      out =  "* #{list[person]["Name"]}"
    end
    out << "\n  * IRC - #{list[person]["IRC"]}" if list[person].has_key?("IRC")
    out << "\n  * [@#{list[person]["Twitter"]}](https://twitter.com/#{list[person]["Twitter"]})" if list[person].has_key?("Twitter")
    out << "\n  * [#{list[person]["email"]}](mailto:#{list[person]["email"]})" if list[person].has_key?("email")
    out << "\n  * #{list[person]["phone"]}" if list[person].has_key?("phone")
    out << "\n  * [ServerFault](#{list[person]["ServerFault"]})" if list[person].has_key?("ServerFault")
    out
  end

rescue LoadError
  STDERR.puts "\n*** TomlRb not available.\n\n"
end
